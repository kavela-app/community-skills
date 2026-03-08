# Optimistic Update Patterns

> Frontend patterns for optimistic/eager UI updates — snapshot, mutate locally, API in background, revert on failure. Used in sidebar drag-and-drop, group creation, and batch operations.

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Kavela)

**Keywords:** `optimistic updates` `eager updates` `snapshot` `rollback` `revert` `drag and drop` `sidebar` `toast` `sonner` `UX` `performance`

[View on Kavela Marketplace](https://kavela.ai/marketplace/kavela-frontend)

---
# Optimistic Update Patterns

## Core Pattern: Snapshot → Mutate → API → Revert

All optimistic updates in the Kavela frontend follow this pattern:

```tsx
// 1. Snapshot current state (shallow copy)
const snap = snapshotSidebar();

// 2. Mutate local state immediately (user sees instant feedback)
moveSkillLocally(skill, targetGroupId);

// 3. Fire API in background
try {
  await updateSkillGroups(skill.id, targetGroupIds);
  await reloadSidebar(); // Reconcile with server truth
} catch (e) {
  restoreSidebar(snap);  // Revert to pre-mutation state
  toast.error("Failed to move skill. Reverted.");
}
```

## Key Helpers (Sidebar.tsx)

### `snapshotSidebar(): SidebarSnapshot`
Returns shallow copies of `groups`, `groupSkillsMap`, `globalSkills`. Used as the rollback point.

### `restoreSidebar(snap: SidebarSnapshot)`
Restores all three state variables from a snapshot. Called on API failure.

### `reloadSidebar()`
Single function replacing 4 previously duplicated reload blocks. Fetches all skills, groups, and per-group skills from the server, then rebuilds local state. Called after successful API mutations to reconcile.

### `findSkillSourceGroupId(skillId): number | null`
Scans `groupSkillsMap` to find which group a skill belongs to. Returns `null` if the skill is in the global (ungrouped) list.

### `moveSkillLocally(skill, targetGroupId)`
Removes a skill from its source group/global and adds it to the target group/global. Uses functional state updaters (`prev => ...`) so multiple calls compose correctly in React's batching.

## Where Optimistic Updates Are Used

| Operation | Helper | Rollback |
|-----------|--------|----------|
| Drag skill between groups | `moveSkillLocally` | `restoreSidebar(snap)` |
| Create new group | Direct `setGroups` + `setGroupSkillsMap` with temp negative ID | `restoreSidebar(snap)` |
| Batch move (multi-select) | Loop `moveSkillLocally` per skill | `restoreSidebar(snap)` |
| Star/unstar (use-stars.ts) | Direct set toggle | Direct set revert |
| **Marketplace star/unstar** | Toggle `starred` + bump `star_count` | Revert both on catch |

## Create Group: Temp ID Pattern

New groups get a temporary negative ID (`-Date.now()`) that never collides with server-assigned positive IDs. On API success, the temp ID is swapped with the real ID in both `groups` and `groupSkillsMap`.

```tsx
const tempId = -Date.now();
setGroups(prev => [...prev, { ...tempGroup, id: tempId }]);
setGroupSkillsMap(prev => ({ ...prev, [tempId]: [] }));
// After API success:
setGroups(prev => prev.map(g => g.id === tempId ? { ...g, id: result.id } : g));
```

## Marketplace Toggle Pattern (Star, Like, Follow)

For simple boolean toggles with a visible counter, update BOTH the toggle state AND the counter optimistically:

```tsx
const [starred, setStarred] = useState(false);

const handleStar = useCallback(async () => {
  if (!listing) return;
  // 1. Snapshot
  const wasStarred = starred;
  // 2. Mutate immediately
  setStarred(!wasStarred);
  setListing((prev) =>
    prev ? { ...prev, star_count: prev.star_count + (wasStarred ? -1 : 1) } : prev
  );
  try {
    // 3. API in background
    if (wasStarred) await unstarListing(listing.id);
    else await starListing(listing.id);
  } catch {
    // 4. Revert on failure
    setStarred(wasStarred);
    setListing((prev) =>
      prev ? { ...prev, star_count: prev.star_count + (wasStarred ? 1 : -1) } : prev
    );
  }
}, [listing, starred]);
```

**Key rules for toggles:**
- State updates happen BEFORE the `await`
- The `try/catch` only rolls back — no success handling needed
- No loading spinners for toggle actions — instant feedback only

## Preloading Pattern (Modals, Dropdowns)

For data the user WILL need (>50% chance), fetch on page mount — not on interaction:

```tsx
// Preload on page mount — not on modal open
const [installTargets, setInstallTargets] = useState<InstallTarget[]>([]);

useEffect(() => {
  fetchInstallTargets()
    .then(({ targets }) => setInstallTargets(targets))
    .catch(() => {}); // Silent — modal will retry if empty
}, []);

// Pass pre-fetched data as props — modal opens instantly
<InstallModal targets={installTargets} ... />
```

**Key rules:**
- Fetch data the user WILL need, not just what they need NOW
- Pass pre-fetched data as props instead of fetching inside modals/dropdowns
- Never show a spinner inside a dropdown — preload the options

## When to Use Optimistic vs Server-Driven

**Use optimistic updates when:**
- The operation has a high success rate (>99%)
- The UI change is visually jarring if delayed (drag-and-drop, inline creation, toggles)
- The mutation is simple enough to model locally

**Use server-driven (await API → update) when:**
- The operation can fail frequently (e.g., permission checks, validation)
- The server response contains data the client can't predict
- The action is destructive/irreversible (install, delete, archive)
- Initial page load (use `reloadSidebar()`)

## Toast Notifications

All reverts show a `toast.error()` from sonner. The `<Toaster>` is in the root layout (`app/layout.tsx`). Import: `import { toast } from "sonner"`.

## Anti-Patterns

- **Waiting for API before updating UI on toggles** — makes it feel laggy
- **Loading spinners on star/like buttons** — use optimistic toggle instead
- **Fetching data inside modals on open** — preload on page mount
- **Alert boxes for success** — use inline success states instead
- **Re-fetching entire listing after star** — just increment the counter locally

## Related Skills
- See also: **UI Design System** for broader visual/interaction guidelines
- See also: **React Component Patterns** for component structure conventions

## Edge Cases

- **Race conditions**: If two rapid optimistic mutations overlap and the first fails, the snapshot revert may undo both. The background `reloadSidebar()` on success reconciles this.
- **Temp ID droppable**: A newly created group with a temp negative ID can be a drop target, but the API call would fail since the server doesn't know that ID. The optimistic drag would revert with a toast. Acceptable UX since group creation resolves in ~200ms.
