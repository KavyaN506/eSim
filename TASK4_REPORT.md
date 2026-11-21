# FOSSEE eSim Upgradation — Task 4 Report

## 1. Task
Investigate installer issues related to eSim and Ubuntu 25.04, inspect the
installation script, and implement a corrective change.

## 2. Repository and environment
- Repository: FOSSEE/eSim
- Working branch: task4-installers
- Installer inspected: Ubuntu/install-eSim-scripts/install-eSim-25.04.sh
- Local test environment: Ubuntu 26.04.1 LTS
- Bash syntax validation: passed

## 3. Issue identified

In the installKicad() function, the installer checks whether KiCad 8.0
is already installed.

When KiCad 8.0 is detected, the function prints a message and executes
`exit 0`.

The main installation flow calls installKicad() before the remaining
setup functions:
- copyKicadLibrary
- installNghdl
- installSky130Pdk
- createDesktopStartScript

Using `exit 0` terminates the entire installer process, rather than
returning control to the main installation flow. This can cause the
remaining eSim setup steps to be skipped while the process reports success.

## 4. Fix implemented

Changed the command inside installKicad():

Before:
    exit 0

After:
    return 0

The function now returns successfully to its caller, allowing the main
installer to continue with subsequent setup steps.

## 5. Validation performed

- Ran Bash syntax validation using:
  `bash -n Ubuntu/install-eSim-scripts/install-eSim-25.04.sh`
- Validation completed without syntax errors.
- Inspected the Git diff and confirmed the intended one-line change.
- Committed the fix:
  `655cd39f Fix installer exit when KiCad 8 is present`
- Pushed the commit to the GitHub fork on branch task4-installers.

## 6. Testing limitation

The local machine is running Ubuntu 26.04.1 LTS, not Ubuntu 25.04.
The installer was syntax-checked, but a complete eSim installation and
runtime validation on Ubuntu 25.04 have not been performed.

Therefore, this report does not claim that the full installation has
been tested or that all Ubuntu 25.04 dependency issues are resolved.

## 7. Conclusion

The installer contains a control-flow defect that can prematurely
terminate installation when KiCad 8.0 is already present. The defect
was corrected by returning from the function instead of exiting the
whole script. The change is committed and available in the submitted
GitHub branch.
