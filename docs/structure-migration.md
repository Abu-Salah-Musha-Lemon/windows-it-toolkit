# Repository Structure Migration

The original ZIP places all files in the repository root. This guide
maps each original filename to the recommended folder structure.

Use `git mv` so Git can track the rename and move history. Run commands
from the repository root in Git Bash.

## 1. Create folders

``` bash
mkdir -p scripts/windows-update
mkdir -p scripts/system-repair
mkdir -p scripts/diagnostics
mkdir -p scripts/inventory
mkdir -p docs
mkdir -p archive
```

## 2. Move and rename scripts

``` bash
git mv "Full-WindowsUpdate-Cleanup.bat" \
  "scripts/windows-update/full-windows-update-cleanup.bat"

git mv "Smart-WindowsUpdate-Cleanup-v2.0.bat" \
  "scripts/windows-update/smart-windows-update-cleanup-v2.bat"

git mv "TECHNICIAN VERSION (Deep Repair Tool)" \
  "scripts/windows-update/technician-deep-repair.bat"

git mv "Smart-WindowsUpdate-Cleanup.bat" \
  "archive/smart-windows-update-cleanup-v1.bat"

git mv "Enterprise_IT_Tool" \
  "scripts/system-repair/enterprise-it-tool.ps1"

git mv "Office Professional Repair Tool (With Logging)" \
  "scripts/system-repair/windows-maintenance-with-logging.bat"

git mv "IT System Audit BAT Script.bat" \
  "scripts/diagnostics/system-audit.bat"

git mv "Professional IT Inventory Script" \
  "scripts/inventory/system-inventory.ps1"
```

The original files without extensions were classified by inspecting
their contents: `Enterprise_IT_Tool` and
`Professional IT Inventory Script` contain PowerShell; the Technician
and Windows Maintenance files contain Batch commands.

## 3. Move and replace documentation

Move the existing WMI/WinSAT guide:

``` bash
git mv "Windows WMI & WinSAT Performance Guide.md" \
  "docs/windows-wmi-winsat-performance-guide.md"
```

Then replace that file with the revised version supplied in this
documentation package.

Replace the old root `README.md` with the new root README.

The original file `widnows activation details.docx` should **not** be
copied into the new repository as-is. It contains copied conversation
text and generic product-key lists. Use the clean
`docs/windows-activation.md` guide in this package instead. Keep the
original DOCX outside the public repository if you need it for personal
reference.

Add the new `.gitignore` and `CONTRIBUTING.md` from this package.

## 4. Review the final structure

``` text
windows-it-toolkit/
├── scripts/
│   ├── windows-update/
│   │   ├── full-windows-update-cleanup.bat
│   │   ├── smart-windows-update-cleanup-v2.bat
│   │   └── technician-deep-repair.bat
│   ├── system-repair/
│   │   ├── enterprise-it-tool.ps1
│   │   └── windows-maintenance-with-logging.bat
│   ├── diagnostics/
│   │   └── system-audit.bat
│   └── inventory/
│       └── system-inventory.ps1
├── docs/
├── archive/
│   └── smart-windows-update-cleanup-v1.bat
├── .gitignore
├── CONTRIBUTING.md
└── README.md
```

## 5. Check and commit

Review every move and inspect the resulting repository:

``` bash
git status
git diff --stat
git diff -- README.md .gitignore
```

Check that the links in `README.md` point to the new paths. Then commit:

``` bash
git add .
git commit -m "docs: organize toolkit documentation and repository structure"
git push origin main
```

## 6. Important

Moving and renaming files does not improve or validate their code.
Review the scripts separately, especially the Technician Deep Repair
script and Smart Windows Update Cleanup v2. Test changes on a virtual
machine or non-production computer before recommending them to others.
