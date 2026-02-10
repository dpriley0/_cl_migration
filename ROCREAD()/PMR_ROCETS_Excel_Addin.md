# Project Migration Record: ROCETS Excel Add-in Development

**Migration Date:** January 29, 2026  
**Source Project Space:** Excel Add-in Development / VBA Code Protection  
**Project Status:** 🔄 In Active Development (Early Stage)

---

## 1. The Big Picture - Your Vision

**End Goal:** Convert your VBA-based `ROCREAD` User-Defined Function into a professional C# Excel Add-in using Excel-DNA, giving you proper IntelliSense tooltips and protecting your source code through compilation.

**Core Philosophy:**

- Escape the archaic Visual Basic Editor (VBE) for modern development tooling
- Protect intellectual property through compilation (not broken VBA passwords)
- Create professional, shareable tools that look and feel like native Excel functions
- Learn by doing - use this project to get comfortable with VS Code and C#
- Baby steps approach - careful, incremental progress over rushing

**Why This Matters:**

- VBA password protection is fundamentally broken (trivially bypassed)
- The VBE lacks modern IDE features (no real IntelliSense, primitive debugging, no Git)
- Your `ROCREAD` function reads ROCETS output files - critical for your rocket engine modeling workflow
- Sharing tools with NASA colleagues requires protecting proprietary logic
- IntelliSense tooltips make functions discoverable and self-documenting

---

## 2. All Apps/Tools/Services You've Shown Interest In

### Currently Installed ✅

- **VS Code** - Development environment (with extensions installed)
- **.NET SDK 9.0.300** - Runtime/build tools (verified working)
- **C# Dev Kit** (by Microsoft) - Language support extension
- **NuGet Package Manager** (by jmrog) - Package management extension
- **.NET Install Tool** (by Microsoft) - .NET version management extension

### Ready to Use 🚀

- **Excel-DNA** - Framework for creating .xll add-ins (via NuGet, not yet installed in project)
- **dotnet CLI** - Command-line build tools (ready to go)

### The Function Being Converted

- **ROCREAD (VBA)** - Reads ROCETS `.OUT` files and returns variable values to Excel
  - Parameters: `varName` (string), `frmt` (optional integer, default 0)
  - Reads config from a `Config` worksheet (folder path, filename, volatility setting)
  - Returns: Double value, formatted string, or error message
  - Format options: 0=raw double, 1=fixed notation, 2=scientific notation

### Future Possibilities (Mentioned/Implied)

- **RefreshSheet Macro** - Convert the `Application.CalculateFull` sub to C#
- **Additional ROCETS UDFs** - Batch reading, array functions, file validation
- **Custom Ribbon** - Add a ROCETS tab to Excel
- **Math.NET Numerics** - Advanced math library for future UDFs
- **Fortran Interop** - P/Invoke to existing Fortran code if needed

---

## 3. Why Excel-DNA? (Decision History)

**The Problem:** You wanted to share your `ROCREAD` UDF with colleagues while:
1. Protecting your source code
2. Getting proper IntelliSense tooltips in Excel

**Options Considered:**

| Option | Verdict | Why |
|--------|---------|-----|
| **VBA Password Protection** | ❌ REJECTED | Fundamentally broken - bypassed with hex editors, file manipulation, or free tools. Password complexity is irrelevant because attackers don't crack the password, they remove it entirely. |
| **Raw XLL with C/C++** | ❌ REJECTED | Too complex - requires deep understanding of Excel C API |
| **COM Add-ins** | ⏸️ Considered | Similar benefits to Excel-DNA but less documentation |
| **Doneex XCell Compiler** | ❌ REJECTED | Commercial, unclear capabilities, vendor dependency |
| **Excel-DNA with C#** | ✅ SELECTED | Free, open-source, great documentation, similar enough to VBA for transition, widely used in finance industry |

**Key Insight from Our Discussions:**

> "VBA password protection is like having an extremely sophisticated lock on your front door while leaving your windows wide open - attackers simply avoid the security measure altogether."

The only real protection is compiled code (XLL/DLL) where the source isn't present at all.

---

## 4. Current State

### Development Environment

- **IDE:** VS Code
- **SDK:** .NET 9.0.300 ✅ (verified with `dotnet --version`)
- **Extensions Installed:**
  - C# Dev Kit (Microsoft) ✅
  - NuGet Package Manager (jmrog) ✅
  - .NET Install Tool (Microsoft) ✅

### Project Creation Status

- **Directory:** Not yet created
- **Project File:** Not yet created
- **Excel-DNA Package:** Not yet installed
- **C# Code:** Not yet written

**You stopped at:** Step 2 of the walkthrough - about to create the project directory and initialize the .NET class library.

### Original VBA Code (Reference)

```vba
Option Explicit
' Dan Riley, ER12
' 2025-03-28

Public Sub RefreshSheet()
    Application.CalculateFull
End Sub

Public Function ROCREAD(varName As String, Optional frmt As Integer = 0) As Variant
    Dim modelDir As String, fileName As String, filePath As String, textLine As String
    Dim fnum As Integer
    Dim parts As Variant
    Dim myVal As Double
    
    ' Volatility setting from Config sheet
    If Worksheets("Config").Range("C8").Value = "ON" Then
        Application.Volatile
    End If
    
    ' Read folder path and filename from Config sheet
    modelDir = Worksheets("Config").Range("C5").Value
    fileName = Worksheets("Config").Range("C6").Value
    
    ' Format path
    If Right(modelDir, 1) <> "\" Then
        modelDir = modelDir & "\"
    End If
    filePath = modelDir & fileName
    
    If Dir(filePath) = "" Then
        ROCREAD = "ERROR: File not found"
        Exit Function
    End If
    
    fnum = FreeFile
    On Error GoTo BugOut
    Open filePath For Input As #fnum
    ROCREAD = "ERROR: variable not found"
    
    Do While Not EOF(fnum)
        Line Input #fnum, textLine
        parts = Split(Application.Trim(Replace(textLine, vbTab, " ")))
        If UBound(parts) >= 1 Then
            If UCase(parts(1)) = UCase(varName) Then
                myVal = CDbl(parts(UBound(parts)))
                Select Case frmt
                    Case 0: ROCREAD = myVal
                    Case 1: ROCREAD = Format$(myVal, "0.0000000")
                    Case 2: ROCREAD = Format$(myVal, "0.0000000E+00")
                    Case Else: ROCREAD = "Error: '" & frmt & "' is not a valid formatting option"
                End Select
                Exit Do
            End If
        End If
    Loop
    
BugOut:
    If fnum <> 0 Then Close #fnum
End Function
```

---

## 5. Known Issues & Open Questions 🔴

### Unresolved Questions

1. **32-bit vs 64-bit Excel** - We haven't confirmed which architecture your Excel installation uses. This matters because the `.xll` must match.

2. **Threat Model Unclear** - I asked what level of protection you need (casual copying vs. professional reverse-engineering vs. internal IP protection) but we moved forward before you answered. Good to clarify for future decisions.

3. **Distribution Method** - How will you share the add-in with colleagues? (Network drive, email, internal portal?)

4. **Additional UDFs** - Are there other VBA functions you want to convert beyond ROCREAD?

### Potential Gotchas

- **Worksheet Dependency** - Current design requires a `Config` worksheet in the workbook. This creates tight coupling.
- **.NET Runtime** - End users may need .NET runtime installed (architecture-dependent)
- **Windows Only** - Excel-DNA only works with desktop Windows Excel (not Mac, not Excel Online)

---

## 6. What's Working Right Now ✅

1. ✅ VS Code installed and configured
2. ✅ .NET SDK 9.0.300 installed and verified
3. ✅ C# Dev Kit extension installed
4. ✅ NuGet Package Manager extension installed
5. ✅ .NET Install Tool extension installed
6. ✅ Original VBA code documented and ready for translation
7. ✅ Architecture decision made (Excel-DNA with C#)

---

## 7. Ready to Do Next 🚀

### Resume Here: Step 2 - Create the Excel-DNA Project

1. **Open VS Code terminal** and navigate to desired location:
   ```bash
   cd C:\Users\[YourUsername]\Documents
   ```

2. **Create project directory:**
   ```bash
   mkdir ROCETSExcelAddin
   cd ROCETSExcelAddin
   ```

3. **Initialize the .NET class library:**
   ```bash
   dotnet new classlib
   ```

4. **Open in VS Code:**
   ```bash
   code .
   ```

5. **Install Excel-DNA package:**
   ```bash
   dotnet add package ExcelDna.AddIn
   ```

**What you should see after Step 5:**
- Explorer panel showing `ROCETSExcelAddin.csproj`, `Class1.cs`, and an `obj` folder
- Terminal showing successful package installation

### Then Continue With:

6. Configure `.csproj` for Excel-DNA settings
7. Replace `Class1.cs` with `ROCETSFunctions.cs`
8. Translate VBA `ROCREAD` logic to C#
9. Add `[ExcelFunction]` and `[ExcelArgument]` attributes for IntelliSense
10. Build with `dotnet build`
11. Test in Excel
12. Troubleshoot and refine

---

## 8. Feature Backlog

### Active Development

- [ ] Create project directory and initialize .NET class library
- [ ] Install Excel-DNA NuGet package
- [ ] Configure `.csproj` for Excel-DNA
- [ ] Translate ROCREAD function to C#
- [ ] Add IntelliSense attributes (function description)
- [ ] Add IntelliSense attributes (parameter hints)
- [ ] Implement error handling
- [ ] Build and test `.xll` file

### Future Enhancements

- [ ] Convert RefreshSheet macro to C#
- [ ] Add custom function category in Excel
- [ ] Create user documentation
- [ ] ROCREAD_BATCH - Array function for multiple variables
- [ ] File validation UDF
- [ ] Custom Excel ribbon for ROCETS tools

### Learning Objectives (Related)

- [ ] Get comfortable with VS Code
- [ ] Learn C# basics
- [ ] Understand .NET project structure
- [ ] Learn Excel-DNA attribute system

---

## 9. Additional Context

### Your Work Environment

- **Employer:** NASA (ER12)
- **Job:** Rocket engine simulation software development
- **Primary Tool:** ROCETS (ROCket Engine Transient Simulator) - Fortran-based
- **Experience:** 13 years with ROCETS, limited Fortran fluency
- **This matters because:** The ROCREAD function specifically parses ROCETS output files

### Your Technical Background

- **Role:** Aerospace Engineer
- **Fortran:** 13 years, limited fluency
- **Python:** Novice (2.0-2.5 out of 5, self-assessed)
- **VBA:** Comfortable (wrote the original ROCREAD)
- **C#:** New (this project is first exposure)
- **VS Code:** Near-zero experience, historically frustrating
- **OOP:** Still learning the paradigm
- **Learning style:** Examples, metaphors, step-by-step guidance

### Preferences for Responses

- TL;DR at the start of complex responses
- TL;DR for each section in multi-section responses
- Reprint instructions after clarifying questions/debugging
- Baby steps - don't rush
- Capitalizes "Project" when referring to Claude Projects
- Wants to understand "why" not just "how"

### Notable Quotes from Our Conversations

On the VBE:
> "And if you CAN'T STAND(!) the Visual Basic Editor (VBE), am I right?!!"

On VS Code:
> "I have near-zero experience with VS Code and haven't had a fun time whenever I've used it, but I really do need to start getting comfortable with it."

---

## 10. What We've Accomplished So Far ✅

1. ✅ Explored VBA code protection options
2. ✅ Learned why VBA password protection is broken
3. ✅ Selected Excel-DNA as the framework
4. ✅ Learned what Excel-DNA can do
5. ✅ Set up VS Code with required extensions
6. ✅ Verified .NET SDK installation (9.0.300)
7. ✅ Documented the original VBA code
8. ✅ Created step-by-step implementation plan

---

## 11. Source Conversations

| Conversation | URL | Key Content |
|--------------|-----|-------------|
| Excel VBA Code Protection Strategy | https://claude.ai/chat/fc55cd0c-9bee-42a3-bbb6-b6622161f07d | Problem definition, protection options analysis, decision to use Excel-DNA |
| Tell me about Excel-DNA | https://claude.ai/chat/63d4d798-8f3e-4448-a268-83d649809894 | Excel-DNA capabilities overview, VBE frustrations |
| Excel UDF IntelliSense Tooltip Conversion | https://claude.ai/chat/4f9b28a3-7dbf-40ea-836b-3139c8b2945a | Active implementation walkthrough (in progress) |

---

## Copy This to Your New Conversation!

When you start fresh, you can reference:

- "We're converting my VBA ROCREAD function to C# using Excel-DNA"
- "VS Code is set up with .NET 9.0.300"
- "I stopped at Step 2 - creating the project directory"
- "I need baby steps - I'm new to VS Code and C#"
- "The goal is IntelliSense tooltips + code protection"

**Pattern is established:**

1. Create .NET class library project
2. Add Excel-DNA NuGet package
3. Write C# function with Excel-DNA attributes
4. Build → get `.xll` file
5. Load add-in in Excel
6. Enjoy proper IntelliSense! 🎉

---

**This foundation is solid - you've made the key architectural decisions and have a clear path forward. Everything from here is just following the established pattern!** 🚀
