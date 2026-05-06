# [BUG] Some tasks are not added in queue list

## Objective

Fix the issue where fast-executing tasks are not appearing in the queue list in the right-menu.tsx component. The root cause is in the tasksconverter store where activeTasks were not being updated when tasks were added.

## Scope

- Modify tasksconverter store to update both global/export/import tasks arrays AND activeTasks when tasks are added
- Ensure all task addition functions include the active tasks update logic
- Update removal functions to also remove from activeTasks
- Modify handleTasksList to merge server tasks with local-only tasks

## User Input Summary

The user reported that in some pages, when a task is triggered, it doesn't appear in the queue list (right-menu.tsx). The component fetches active tasks from Taskconverterstore, but the logic was flawed: active tasks were extracted after a Redis request, and fast-completing tasks didn't get listed because the polling didn't capture them in time.

## Technical Plan

1. ✅ Modified `addExportTask`, `addImportTask`, `addGlobalTask` to add task to activeTasks simultaneously
2. ✅ Modified `removeExportTask`, `removeImportTask`, `removeGlobalTask` to remove task from activeTasks
3. ✅ Updated `setActiveTasks` to support functional updates
4. ✅ Modified `handleTasksList` in use-case-polling.ts to merge server tasks with local-only tasks
5. ✅ Exported `ActiveItem` type from use-task-store for use in use-case-polling.ts

## Affected Files

- `src/hooks/tasks-converter/use-task-store.ts`
- `src/hooks/cases/use-case-polling.ts`

## Risks / Notes

- Changes maintain backward compatibility (taskLabel is optional)
- Local tasks that haven't hit the server yet will be preserved during polling sync