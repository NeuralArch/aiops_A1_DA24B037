# AI Disclosure

## 1. Which tools were used
- Claude (Anthropic)

## 2. How they were used
- **Debugging and troubleshooting:** Pasted terminal error logs (e.g., `dvc push` failing with `ERROR: unexpected error - ssh is supported, but requires 'dvc-ssh' to be installed`, and `dvc checkout` showing deleted files instead of restored ones) to Claude to understand the root cause and identify fixes. This was used to resolve DVC remote/cache state issues during the Q3 data-versioning workflow.

- **Video planning:** Asked Claude to help for guidance on trimming/splitting recorded footage in Canva's video editor to fit a 4–5 minute time limit.

- **Readme file:** Used claude assistance while making the Readme.md file to tell about the repository.

## 3. Impact
AI assistance was used for troubleshooting execution errors (DVC remote/SSH config issues), for organizing the demonstration/recording workflow. All core ML training code, DVC data-versioning commands, and MLflow experiment logic were written and executed by me; AI was not used to generate the underlying `train.py` model training logic, the six-run MLflow experiment loop, or the DVC data-versioning design — only to debug errors encountered while running them and to help plan the accompanying video explanation.

