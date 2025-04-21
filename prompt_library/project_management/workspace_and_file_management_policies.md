# Workspace, Directory, and File Management Policies

## Workspace Directory Behavior
- The current workspace directory is the default for all tool operations and new terminals.
- Changing directories in a terminal does not affect the workspace directory for tools.
- When a new task begins, a recursive list of all filepaths in the workspace is provided for project overview.
- Use this overview to guide decisions about file exploration and code organization.

## Directory and File Listing
- Use the list_files tool to explore directories, especially outside the workspace.
- For nested structures, use recursive listing; for top-level exploration, use non-recursive.
- Avoid creating new project directories unless explicitly requested by the user.

## File and Directory Naming
- Reference file and directory names as they appear in the workspace overview.
- Use file extensions and directory structure to infer language and project organization.

## Best Practices
- Always prefer using provided tools for file and directory operations over manual shell commands.
- Prioritize clarity and maintainability in file organization.
- Document any changes to workspace structure clearly in progress reports or changelogs.
