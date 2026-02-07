# AGENTS.md

This file contains guidelines for agentic coding agents working with the Ventoy configuration repository.

## Project Overview

This repository contains Ventoy configuration files and Windows unattended installation scripts. It's not a traditional software project with source code, but rather a configuration package that enables automated Windows installations through Ventoy.

### Repository Structure
```
Ventoy/
├── LICENSE
└── ventoy/
    ├── ventoy.json           # Main Ventoy configuration
    └── script/
        └── Windows_10_11/    # Windows unattended installation XML files
```

## Build/Test/Lint Commands

This repository contains only configuration files and does not have a traditional build system. There are no compilation, testing, or linting commands required.

## Validation Procedures

Since this repository contains configuration files, use these validation approaches:

### JSON Configuration Validation
```bash
# Validate ventoy.json syntax
python -m json.tool /path/to/ventoy.json
```

### XML Answer File Validation
```bash
# Validate XML syntax for Windows answer files
xmllint --noout /path/to/script/Windows_10_11/*.xml
```

### Ventoy Integration Testing
1. Copy repository contents to Ventoy USB drive
2. Boot with Ventoy and verify auto-installation options appear
3. Test Windows installation with a virtual machine

## Code Style Guidelines

### JSON Configuration (ventoy.json)
- Use 4 spaces for indentation
- Maintain alphabetical ordering where appropriate (control array)
- Keep configuration objects in logical groups
- Use double quotes for all keys and string values

#### Example Structure:
```json
{
  "auto_install": [
    {
      "autosel": 0,
      "parent": "/ISO/Windows",
      "template": [
        "/ventoy/script/Windows_10_11/pro.xml"
      ],
      "timeout": 15
    }
  ],
  "control": [
    {
      "VTOY_DEFAULT_IMAGE": "VTOY_EXIT"
    }
  ]
}
```

### XML Answer Files (Windows_10_11/*.xml)
- Follow Microsoft's Answer File schema (urn:schemas-microsoft-com:unattend)
- Use UTF-8 encoding
- Include proper XML declaration
- Structure components by pass (windowsPE, specialize, etc.)
- Use meaningful component names matching Microsoft standards

#### Example Structure:
```xml
<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend">
    <settings pass="windowsPE">
        <component name="Microsoft-Windows-Setup" processorArchitecture="amd64">
            <UserData>
                <AcceptEula>true</AcceptEula>
            </UserData>
        </component>
    </settings>
</unattend>
```

## File Naming Conventions

### XML Answer Files
- Use lowercase with underscores: `pro.xml`, `enterprise_n.xml`
- Include edition in filename: `pro`, `enterprise`, `home`, `education`
- Add variant suffixes: `_n` for N editions, `_g` for G editions
- Describe special features: `_4_workstations`, `_4_virtual_desktops`

### Directory Structure
- Use lowercase for directory names
- Group scripts by OS version: `Windows_10_11/`
- Keep scripts in logical hierarchy

## Configuration Standards

### Ventoy Configuration Keys
- Maintain all VTOY_ prefixes for Ventoy-specific settings
- Use descriptive key names following Ventoy documentation
- Include comments for non-obvious configuration values

### Auto-installation Templates
- Include all necessary Windows editions for completeness
- Maintain consistent timeout values (default: 15 seconds)
- Use relative paths from Ventoy root directory

## Modification Guidelines

1. **Adding New Windows Editions**
   - Create new XML file following naming convention
   - Include appropriate product key
   - Add to ventoy.json template array
   - Test with VM before deployment

2. **Modifying Ventoy Settings**
   - Understand Ventoy documentation before changes
   - Test boot behavior after modifications
   - Keep backup of working configuration

3. **Product Key Management**
   - Only use legitimate product keys
   - Consider using generic keys for testing
   - Document key sources if needed

## Security Considerations

1. Product keys should be legitimate and properly licensed
2. Test configurations in isolated VMs before deployment
3. Be cautious with bypass settings (VTOY_WIN11_BYPASS_CHECK)
4. Review all scripts before production use

## Common Tasks

### Adding New Auto-installation Template
1. Create XML file in appropriate directory
2. Follow Microsoft Answer File schema
3. Add file path to ventoy.json template array
4. Validate XML syntax
5. Test with appropriate Windows ISO

### Modifying Boot Behavior
1. Edit ventoy.json control array
2. Consult Ventoy documentation for key meanings
3. Validate JSON syntax
4. Test boot behavior on USB drive

## Resources

- [Ventoy Official Documentation](https://www.ventoy.net/en/doc.html)
- [Windows Answer File Technical Reference](https://docs.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/)
- [Windows ADK for Answer File Creation](https://docs.microsoft.com/en-us/windows-hardware/get-started/adk-install)