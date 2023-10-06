# HDiffPatch_progress
![hdiffpatch_mod](https://github.com/mgrinzPlayer/HDiffPatch_progress/assets/9157726/32b5b9c1-674f-4a36-b354-faba8047747f)

Modifications:
- -v-level  verbose level. 0=none 1=patchOK 2=+time 3=all
- -p[-type]  enable show progress. type 1=print 2=title 3=both(default);
               checks HDIFFPATCH_PRINT and HDIFFPATCH_TITLE environment variables;
               by default it uses this sprintf "Progress %.3f%%" string.

Automatic parent folder creation. For example:
`hpatchz OLDFILE.pak patchFILE notexistingdir\NEWFILE.pak`
![obraz](https://github.com/mgrinzPlayer/HDiffPatch_progress/assets/9157726/9e0c3af8-d763-4457-834f-c7e7c44392d1)

In this repo, releases are made automatically with GitHub Actions (just read workflow from .github/workflows/compileandupload.yml):
- msys2/setup-msys2@v2 - installs MSYS2 and mingw-w64 tools
- compiled binaries are compressed to tar.xz archive. SHA1 checksum is computed. binaries sources and SHA1 checksum file are uploaded as artifacts
- softprops/action-gh-release@v1 - creates release, archives are uploaded as assets.
- to be sure binaries.tar.xz asset is the original workflow run archive, you can compare released binaries.tar.xz SHA1 checksum with file from workflow artifacts
- (its name contains SHA1 checksum too, because it was uploaded with `name: checksum-${{ steps.computedHash.outputs.sha1sumhash }}`)

Build process:
- Makefile.patch - compatibility MSYS2 and mingw-w64-x86_64-gcc (also i686)
- progress+make_parent_directories_as_needed+verboselevel.patch - applies modifications mentioned earlier
- make flags: ZLIB=1 BZIP2=1 STATIC_CPP=1 MINS=1

All modules are based on the github.com/sisong repositories. All pinpoint to commits used to build original HDiffPatch v4.6.3:
- HDiffPatch @ b2555ed
- bzip2 @ 9de658d
- libmd5 @ 51edeb6
- lzma @ af82ecb
- zlib @ cacf7f1
- zstd @ db2ba8f
