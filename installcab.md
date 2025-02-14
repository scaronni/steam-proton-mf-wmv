# python-installcab

Extract and install components from cab based installers

This tool is intended to be used with [wine](https://winehq.org)

## Usage:

```
python installcab.py [options] cabfile filter [wineprefix_path]
```

- cabfile: an exe installer or cab file that can be extracted with cabextract
- filter: a filter for the components inside the cab file (will match anywhere in available files inside the cabinet)
- wineprefix_path: you can set this, otherwise it will try to get from your WINEPREFIX environment variable

### Options

-  --nocleanup: do not perform cleanup after running
-  --noreg: do not import registry entries
-  --nodll: do not install dlls into system dir
-  --register: register dlls with regsrv32
-  --stripdllpath: strip full path for dlls in registry so wine can find them through it's own resolver
   
## Examples

```
python installcab.py ~/.cache/winetricks/win7sp1/windows6.1-KB976932-X86.exe x86_microsoft-windows-mediafoundation"
```

will extract and install any manifest files and dlls with 'x86_microsoft-windows-mediafoundation' in their path.

```
python installcab.py ~/.cache/winetricks/win7sp1/windows6.1-KB976932-X64.exe wmadmod
```

will extract and install any manifest files and dlls with 'wmadmod' in their path.

## What exactly it does

1. Extracts from the cab file using cabextract with 'component' as filter
2. Finds out what dll files and manifest files where extracted
3. Will place dll files in $WINEPREFIX/windows/system32/ or syswow64 depending on arch
4. Will convert manifest data to .reg file and import it through wine/wine64

The software doesn't try to find out dependencies or be smart about where to install dlls, it will just install dlls in the system32 or syswow64 directory, and import stuff into wine's registry from manifest files.

It does not aim to fully support cabinet installers, but is capable of installing just parts from an installer, going a bit further than what is actually possible with 'proper' support.

## License

This software is released into the Public Domain by use of the Unlicense, see the [LICENSE](LICENSE) file for more details.
