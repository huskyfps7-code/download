#define MyAppName "HNLPB"
#define MyAppVersion "10.4"
#define MyAppPublisher "Husky FPS"
#define MyAppURL "https://www.huskyfps.pro/"
#define MyAppExeName "HNLPB.exe"

[Setup]
AppId={{0E189FC2-D062-4E7F-BA96-95F35222DAA8}}
AppName={#MyAppName}
AppVersion={#MyAppVersion}
AppPublisher={#MyAppPublisher}
AppPublisherURL={#MyAppURL}
AppSupportURL={#MyAppURL}
DefaultDirName={autopf}\{#MyAppName}
UninstallDisplayIcon={app}\{#MyAppExeName}
ChangesAssociations=yes
DisableProgramGroupPage=yes
PrivilegesRequired=admin
OutputDir=D:\
OutputBaseFilename={#MyAppName} {#MyAppVersion}
SetupIconFile=C:\Users\dedex\Documents\Corel\favicon.ico
SolidCompression=yes
WizardStyle=modern

[Languages]
Name: "english"; MessagesFile: "compiler:Default.isl"
Name: "brazilianportuguese"; MessagesFile: "compiler:Languages\BrazilianPortuguese.isl"

[Tasks]
Name: "desktopicon"; Description: "{cm:CreateDesktopIcon}"; GroupDescription: "{cm:AdditionalIcons}"; Flags: unchecked

[Files]
Source: "A:\HNLPB\{#MyAppExeName}"; DestDir: "{app}"; Flags: ignoreversion
Source: "A:\HNLPB\*"; DestDir: "{app}"; Flags: ignoreversion recursesubdirs createallsubdirs

[Registry]
Root: HKCR; Subkey: "Software\Classes\.exe\OpenWithProgids"; ValueType: string; ValueName: "HNLPBexe"; ValueData: ""; Flags: uninsdeletevalue
Root: HKCR; Subkey: "Software\Classes\HNLPBexe"; ValueType: string; ValueName: ""; ValueData: "HNLPB File"; Flags: uninsdeletekey
Root: HKCR; Subkey: "Software\Classes\HNLPBexe\DefaultIcon"; ValueType: string; ValueName: ""; ValueData: "{app}\{#MyAppExeName},0"
Root: HKCR; Subkey: "Software\Classes\HNLPBexe\shell\open\command"; ValueType: string; ValueName: ""; ValueData: """{app}\{#MyAppExeName}"" ""%1"""
Root: HKCU; Subkey: "Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers"; \
    ValueType: string; ValueName: "{app}\{#MyAppExeName}"; ValueData: "~ RUNASADMIN"; Flags: uninsdeletevalue

[Icons]
Name: "{autoprograms}\{#MyAppName}"; Filename: "{app}\{#MyAppExeName}"
Name: "{autodesktop}\{#MyAppName}"; Filename: "{app}\{#MyAppExeName}"; Tasks: desktopicon

[Run]
Filename: "{app}\{#MyAppExeName}"; Description: "{cm:LaunchProgram,{#StringChange(MyAppName, '&', '&&')}}"; Flags: nowait postinstall skipifsilent

[Code]
function IsAppRunning(const FileName: String): Boolean;
var
  ResultCode: Integer;
begin
  // Usa o tasklist para verificar se o processo está rodando
  Result := (Exec('cmd.exe', '/C tasklist /FI "IMAGENAME eq ' + FileName + '" | find /I "' + FileName + '"', '', SW_HIDE, ewWaitUntilTerminated, ResultCode) and (ResultCode = 0));
end;

procedure KillProcess(const FileName: String);
var
  ResultCode: Integer;
begin
  // Usa o taskkill para encerrar o processo forçadamente
  Exec('cmd.exe', '/C taskkill /F /IM "' + FileName + '"', '', SW_HIDE, ewWaitUntilTerminated, ResultCode);
end;

procedure InitializeWizard();
begin
  if IsAppRunning('{#MyAppExeName}') then
  begin
    MsgBox('{#MyAppName} está aberto e será fechado para continuar a instalação.', mbInformation, MB_OK);
    KillProcess('{#MyAppExeName}');
    Sleep(1000); // espera 1 segundo
  end;
end;
 
