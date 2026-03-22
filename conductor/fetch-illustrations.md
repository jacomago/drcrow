# Fetch Illustrations Plan

## Objective
Fetch the illustrations from the `gh-pages-templated` branch and place them into an `illustrations` folder on the current branch.

## Implementation Steps
1. **Identify Files**: Check the exact name of the branch (`gh-pages-templated` or `gh-page-templated`) and the target folder (`img`).
2. **Fetch Files**: Use `git checkout` or `git restore` to bring the `img` folder from that branch into the current working directory without switching branches.
3. **Organize Files**: Move the fetched `img` directory to `assets/illustrations` (which fits the Jekyll structure) or `illustrations` at the root, depending on the files.
4. **Integration (Optional)**: If desired, we can then update `about.md` to reference these new illustrations in the EXPOSITION section.

## Verification
- Run `ls illustrations` or `ls assets/illustrations` to ensure the files were copied correctly.
- Ensure the git status shows the newly added files as untracked, ready to be committed.
