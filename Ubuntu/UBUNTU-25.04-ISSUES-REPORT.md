# eSim on Ubuntu 25.04 (WSL2) - Issues Report

Environment: Ubuntu 25.04 on WSL2 (VirtualBox failed: graphics/input problems, low RAM, no C: space).

| # | Issue | Cause | Fix | Status |
|---|-------|-------|-----|--------|
| 1 | install-eSim.sh syntax error near } | Unclosed if, stray config lines in dispatcher | Fixed dispatcher (commit 5ffa25b0) | Fixed |
| 2 | Detected Ubuntu Version empty | grep expects 3-part version (x.y.z); 25.04 has 2 parts | Regex should be \d+\.\d+(\.\d+)? | Reported |
| 3 | KiCad PPA 503 Service Unavailable | PPA temporarily down, no retry | Reported | Reported |
| 4 | KiCad PPA 8.0.9 needs libgit2-1.8 (unavailable on 25.04) | PPA build incompatible with 25.04 | Skip PPA on 25.04, use Ubuntu archive KiCad 8.0.8 (commit 66a1d976) | Fixed |
| 5 | Script exited after KiCad step, eSim not installed | Early exit in KiCad "already installed" branch | return 0 instead of exit | Fixed |
| 6 | cp: /usr/share/kicad/symbols/ not found | kicad-symbols/footprints/templates/libraries not installed | Installed those packages | Workaround |
| 7 | NGHDL: Unsupported Ubuntu version: 25.04 | No 25.04 case in install-nghdl.sh, no install-nghdl-25.04.sh | Added "25.04" case + 25.04 script (copy of 24.04) | Fixed |
| 8 | NGHDL patch lost on every install | install script runs unzip -o nghdl.zip, overwriting patched files | Updated patched files inside nghdl.zip | Fixed |
| 9 | libcanberra-gtk-module has no installation candidate | GTK2 module removed in 25.04 | Removed from install-nghdl-25.04.sh, kept gtk3 module | Fixed |
| 10 | NGHDL: GHDL 4.1.0 configure fails: Unhandled version llvm 20.1.2 (default) and llvm 19.1.7 (after installing llvm-19) | Bundled GHDL 4.1.0 configure does not recognise LLVM 19/20 shipped in Ubuntu 25.04 | Needs LLVM 18 or older, or newer GHDL (4.2+/5.x) | Identified, not fixed |

Notes:
- KiCad PPA fix (#4) was verified by logic and git diff; KiCad 8.0.8 was already installed manually, so the script took the "already installed" path.
- NGHDL steps now get past version detection and dependency install (#7, #8, #9) and fail at GHDL configure (#10).
