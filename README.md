# WinRAR Activator - 🄯 Ashesh Development

A local Windows script with a selectable registration name and the fixed
organization **🄯 Ashesh Development**. The default name is **Erich Alfred Bossert**.

Inspired by and adapted from the original
[WinRAR Activator](https://github.com/NaeemBolchhi/WinRAR-Activator), created by
[@NaeemBolchhi](https://github.com/NaeemBolchhi). This version adds registration
name selection, dynamic key generation, and backup handling.

Free to use, modify, and redistribute under the [GNU GPL version 3](LICENSE).

## Features

- Interactive name selection when activating, or a `-Name` / `--name` option.
- Fresh registration data generated for each selected name.
- Fixed organization text: `🄯 Ashesh Development`.
- Local generation with a bundled, checksum-verified tool and no runtime downloads.
- Preview and export modes that run without administrator privileges.
- Automatic discovery of Program Files and current-user Scoop installations.
- Backup of an existing registration file before replacement.

The console displays:

```text
WinRAR Activator v0.02 - 🄯 Ashesh Development
Registration name: Erich Alfred Bossert
🄯 Ashesh Development
```

## Requirements

- Windows 10 or Windows 11.
- Windows PowerShell 5.1 or later.
- An existing WinRAR installation for installation mode.

No compilation or dependency installation is required. Keep `WRA.cmd`,
`WRA.ps1`, `src/`, and `tools/` together. The batch file launches PowerShell;
it requires the accompanying files.

## Usage

Open PowerShell in the repository folder. Preview a chosen registration name
without writing files or requesting elevation:

```powershell
.\WRA.cmd --preview --name "Your Registration Name"
```

Export newly generated data for a custom installation. The destination folder must
exist, and the output filename must be unused:

```powershell
.\WRA.cmd --name "Your Registration Name" --export "$env:TEMP\Ashesh-rarreg.key"
```

To activate, close WinRAR and run:

```powershell
.\WRA.cmd
```

Enter a registration name when prompted, or press Enter to keep
`Erich Alfred Bossert`. To skip the prompt and use a specific name, run:

```powershell
.\WRA.cmd --name "Your Registration Name"
```

Installation mode requests administrator privileges and preserves the selected
name when relaunching with elevation. After installation, restart
WinRAR and inspect **Help > About WinRAR** to check whether the key is recognized.
The script reports whether the file was installed; application recognition
remains untested.

### PowerShell

Run these commands from the repository folder. For interactive name selection:

```powershell
powershell.exe -NoProfile -File .\WRA.ps1
```

To supply a name without the prompt:

```powershell
powershell.exe -NoProfile -File .\WRA.ps1 -Name "Your Registration Name"
```

For preview, export, or help, choose the relevant command:

```powershell
powershell.exe -NoProfile -File .\WRA.ps1 -Preview -Name "Your Registration Name"
powershell.exe -NoProfile -File .\WRA.ps1 -ExportPath "$env:TEMP\Ashesh-rarreg.key" -Name "Your Registration Name"
powershell.exe -NoProfile -File .\WRA.ps1 -Help
```

Use `.\WRA.cmd --help` for batch options.

Preview and export use the default name if no name is supplied. Names may include
spaces and Unicode characters; quote names on the command line. Use the prompt
or PowerShell entry point for names containing shell punctuation. Blank names,
control characters, and names starting with a hyphen are rejected. The
organization has no override option.

`Resolve-RegistrationName` in `src/Registration.psm1` handles name selection;
`New-RegistrationData` generates matching data using the fixed organization.
The generated key uses UTF-8 without a BOM. Its organization line starts with
`utf8:` as required by the generator's Unicode format; the organization value
is `🄯 Ashesh Development`. A Unicode-capable console font is needed to render
the symbol.

## Backup and Restore

Existing registration data is saved beside the installed key as
`rarreg.key.<guid>.bak`. To restore it, close WinRAR and replace
`rarreg.key` with the selected backup, retaining the name `rarreg.key`.

## Repository Files

| File | Purpose |
| --- | --- |
| [WRA.cmd](WRA.cmd) | Batch entry point for installation, preview, and export. |
| [WRA.ps1](WRA.ps1) | Command options and interactive activation. |
| [src/Registration.psm1](src/Registration.psm1) | Name selection, generation, export, and backup functions. |
| [tools/winrar-keygen/README.md](tools/winrar-keygen/README.md) | Pinned generator, source location, checksum, and MIT license. |
| [rarreg.key](rarreg.key) | Default-name sample with the fixed organization; custom runs generate fresh data. |
| [tests/Smoke.Tests.ps1](tests/Smoke.Tests.ps1) | Isolated checks using temporary directories. |
| [LICENSE](LICENSE) | Full GNU GPL version 3 terms. |
| [NOTICE](NOTICE) | Attribution and modification notice. |

## Development and Validation

Run the existing checks from the repository root:

```powershell
powershell.exe -NoProfile -File .\tests\Smoke.Tests.ps1
git diff --check
```

Smoke checks cover name selection, quoted and Unicode names, fixed organization,
UTF-8 output, local launching, preview/export, isolated installation, backup
preservation, invalid options, and missing or tampered generator files.
Actual UAC elevation and WinRAR key recognition require
separate manual verification.

## License

The scripts and documentation are released under **GNU GPL version 3
(GPL-3.0-only)**. You may use, study, modify, and redistribute them, including
commercially, under those terms.

When distributing this project or a modified version, preserve attribution and
license notices, include the GPL, and provide the corresponding source under
its terms. Mark your modifications and their dates. The software comes without
warranty; see [LICENSE](LICENSE) for the complete terms and [NOTICE](NOTICE) for
project attribution.

This license covers the project scripts and documentation. The bundled
third-party generator retains its [MIT license](tools/winrar-keygen/LICENSE).
These licenses do not grant a WinRAR license.

## Credits and Provenance

- Original creator: [@NaeemBolchhi](https://github.com/NaeemBolchhi), author of
  [WinRAR Activator](https://github.com/NaeemBolchhi/WinRAR-Activator),
  distributed with GNU GPL version 3.
- Customization: **Erich Alfred Bossert / Ashesh Development**, 2026-11-18.
- Registration data generation: [bitcookies/winrar-keygen v4.3.0](https://github.com/bitcookies/winrar-keygen/releases/tag/v4.3.0),
  bundled with its MIT license.

The scripts generate each key locally with the selected name and fixed
organization. The equivalent generator command is:

```text
winrar-keygen-x86.exe "Your Registration Name" "🄯 Ashesh Development" --text --encoding utf8
```

The bundled x86 executable's SHA-256 is verified before each use:

```text
9b9be01389980dc3a22c995773671364f8823cc5ffab96f9b701a5e6220bbbb2
```
