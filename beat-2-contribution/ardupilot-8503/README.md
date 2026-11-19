# ArduPilot#8503 working files

- `patches/`: the four commits for branch `rover-fs-do-land-start`, based on
  ArduPilot master `5cdd616`. Apply them to a fork with:

      git clone https://github.com/<you>/ardupilot && cd ardupilot
      git checkout -b rover-fs-do-land-start
      git am /path/to/patches/*.patch
      git push -u origin rover-fs-do-land-start

- `logs/`: autotest excerpts from before the fix, after the fix, and the regression summary.
- `pr-description.md`: draft PR body that follows ArduPilot's template.
