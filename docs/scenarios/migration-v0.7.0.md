# Migrating to v0.7.0

This document contains instructions to help migrate from previous versions of PSDocs.

## Migrating from v0.6.x

### Inline blocks

Previously a `Document` block and the command that generated the document could be called inline within the same script.

For example:

```powershell
Document 'SampleMessage' {
    'Testing 123.'
}

Sample -InputObject @{ }
```

Support for inline `Document` blocks has been removed. Use the following steps to migrate inline blocks to a file if you previously used this feature:

- Create a new file ending with `.doc.ps1` file extension. For example `Sample.doc.ps1`.
- Copy and paste the previous inline `Document` block into the file.
- Update command-line to use `Invoke-PSDocument` instead of using the name of the document block directly.

Example `Sample.doc.ps1`:

```powershell
Document 'SampleMessage' {
    'Testing 123.'
}
```

The cmdlet `Invoke-PSDocument` can be called as follows:

```powershell
Invoke-PSDocument -Path .\Sample.doc.ps1 -Name SampleMessage;
```

On Linux, the file extension `.Doc.ps1` is not automatically found by PSDocs because of file system case-sensitivity. For consistency, use `.doc.ps1` on all platforms.

### Script block usage of Note and Warning

Previously the following was possible.

```powershell
Document 'NoteScriptBlock' {
    Note {
        'A note'
    }
}
```

This syntax was deprecated as of v0.6.0, and has been removed as of v0.7.0.
Instead of using a script block, pipe the note or warning text to `Note`, `Warning`.

For example:

```powershell
Document 'NotePipe' {
    'A note' | Note
}
```

### Section -When parameter

Previously the following was possible.

```powershell
Document 'SectionWhen' {
    Section -When { $i -eq 0 } {
        'Sample section text.'
    }
}
```

The `-When` parameter was replaced with `-If` as of v0.6.0, and has been removed as of v0.7.0.
Instead of using `-When`, update section blocks to use `-If`.

For example:

```powershell
Document 'SectionIf' {
    Section -If { $i -eq 0 } {
        'Sample section text.'
    }
}
```