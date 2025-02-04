

local a=game:GetService"Players"



local b=a.LocalPlayer local c=
b.Character or b.CharacterAdded:Wait()



local function LoadInfyield()
loadstring(game:HttpGet"https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source")()
end

local function LoadDebugUI()
loadstring(game:HttpGet"https://raw.githubusercontent.com/tundra8x/walmarthacksuniversal/refs/heads/main/README.md")();
end

local function LoadDex()
loadstring(game:HttpGet"https://raw.githubusercontent.com/infyiff/backup/main/dex.lua")()
end

local function UnloadDebugUI()
b.PlayerGui.Debug:Destroy()
end

local function KillRoblox()
game:Shutdown()
end

local function RejoinSameGame()
local d=game:GetService"TeleportService"
local e=game.PlaceId
local f=game.JobId
d:TeleportToPlaceInstance(e,f,b)
end




local d='https://raw.githubusercontent.com/violin-suzutsuki/LinoriaLib/main/'

local e=loadstring(game:HttpGet(d..'Library.lua'))()
loadstring(game:HttpGet(d..'addons/ThemeManager.lua'))()
loadstring(game:HttpGet(d..'addons/SaveManager.lua'))()



local f=e:CreateWindow{
Title='walmart hacks lul | universal',
Center=true,
AutoShow=true,
TabPadding=8,
MenuFadeTime=0.2
}



local g={
Home=f:AddTab'Home',
Visuals=f:AddTab'Visuals',
Blatant=f:AddTab'Blatant',
Settings=f:AddTab'Settings',
}



_G.ESP=false
_G.NameTags=false
_G.Aimbot=false
_G.SoftAim=false



local h=g.Visuals:AddLeftGroupbox'Visuals'
local i=g.Blatant:AddLeftGroupbox'Blatant'





h:AddToggle('ESP',{
Text='ESP',
Default=false,
Tooltip='Allows you to see people through walls',

Callback=function(j)
_G.ESP=j
end
})

h:AddToggle('NameTags',{
Text='NameTags',
Default=false,
Tooltip='Allows you to see peoples usernames through walls',

Callback=function(j)
_G.NameTags=j
end
})



i:AddToggle('Aimbot',{
Text='Aimbot',
Default=false,
Tooltip='Locks on to the closest players head',

Callback=function(j)
if j==true then
_G.Aimbot=true
end

while wait()do

if _G.Aimbot==false then return end



local k=select local
l, m, n, o, p, q, r=k(1,pcall,getgenv,next,Vector2.new,math.clamp,type,mousemoverel or(Input and Input.MouseMove))



l(function()
m().Aimbot.Functions:Exit()
end)



m().Aimbot={}
local s=m().Aimbot



local t=game:GetService"RunService"
local u=game:GetService"UserInputService"
local v=game:GetService"TweenService"
local w=game:GetService"Players"
local x=workspace.CurrentCamera
local y=w.LocalPlayer



local z,A,B,C,D=2000,false,false,{}



s.Settings={
Enabled=true,
TeamCheck=false,
AliveCheck=true,
WallCheck=false,
Sensitivity=0,
ThirdPerson=false,
ThirdPersonSensitivity=3,
TriggerKey="MouseButton2",
Toggle=false,
LockPart="Head"
}

s.FOVSettings={
Enabled=true,
Visible=true,
Amount=480,
Color=Color3.fromRGB(255,255,255),
LockedColor=Color3.fromRGB(255,70,70),
Transparency=0.5,
Sides=60,
Thickness=1,
Filled=false
}

s.FOVCircle=Drawing.new"Circle"



local function CancelLock()
s.Locked=nil
if D then D:Cancel()end
s.FOVCircle.Color=s.FOVSettings.Color
end

local function GetClosestPlayer()
if not s.Locked then
z=(s.FOVSettings.Enabled and s.FOVSettings.Amount or 2000)

for E,F in n,w:GetPlayers()do
if F~=y then
if F.Character and F.Character:FindFirstChild(s.Settings.LockPart)and F.Character:FindFirstChildOfClass"Humanoid"then
if s.Settings.TeamCheck and F.Team==y.Team then continue end
if s.Settings.AliveCheck and F.Character:FindFirstChildOfClass"Humanoid".Health<=0 then continue end
if s.Settings.WallCheck and#(x:GetPartsObscuringTarget({F.Character[s.Settings.LockPart].Position},F.Character:GetDescendants()))>0 then continue end

local G,H=x:WorldToViewportPoint(F.Character[s.Settings.LockPart].Position)
local I=(o(u:GetMouseLocation().X,u:GetMouseLocation().Y)-o(G.X,G.Y)).Magnitude

if I<z and H then
z=I
s.Locked=F
end
end
end
end
elseif(o(u:GetMouseLocation().X,u:GetMouseLocation().Y)-o(x:WorldToViewportPoint(s.Locked.Character[s.Settings.LockPart].Position).X,x:WorldToViewportPoint(s.Locked.Character[s.Settings.LockPart].Position).Y)).Magnitude>z then
CancelLock()
end
end



C.TypingStartedConnection=u.TextBoxFocused:Connect(function()
A=true
end)

C.TypingEndedConnection=u.TextBoxFocusReleased:Connect(function()
A=false
end)



local function Load()
C.RenderSteppedConnection=t.RenderStepped:Connect(function()
if s.FOVSettings.Enabled and s.Settings.Enabled then
s.FOVCircle.Radius=s.FOVSettings.Amount
s.FOVCircle.Thickness=s.FOVSettings.Thickness
s.FOVCircle.Filled=s.FOVSettings.Filled
s.FOVCircle.NumSides=s.FOVSettings.Sides
s.FOVCircle.Color=s.FOVSettings.Color
s.FOVCircle.Transparency=s.FOVSettings.Transparency
s.FOVCircle.Visible=s.FOVSettings.Visible
s.FOVCircle.Position=o(u:GetMouseLocation().X,u:GetMouseLocation().Y)
else
s.FOVCircle.Visible=false
end

if B and s.Settings.Enabled then
GetClosestPlayer()

if s.Locked then
if s.Settings.ThirdPerson then
s.Settings.ThirdPersonSensitivity=p(s.Settings.ThirdPersonSensitivity,0.1,5)

local E=x:WorldToViewportPoint(s.Locked.Character[s.Settings.LockPart].Position)
r((E.X-u:GetMouseLocation().X)*s.Settings.ThirdPersonSensitivity,(E.Y-u:GetMouseLocation().Y)*s.Settings.ThirdPersonSensitivity)
else
if s.Settings.Sensitivity>0 then
D=v:Create(x,TweenInfo.new(s.Settings.Sensitivity,Enum.EasingStyle.Sine,Enum.EasingDirection.Out),{CFrame=CFrame.new(x.CFrame.Position,s.Locked.Character[s.Settings.LockPart].Position)})
D:Play()
else
x.CFrame=CFrame.new(x.CFrame.Position,s.Locked.Character[s.Settings.LockPart].Position)
end
end

s.FOVCircle.Color=s.FOVSettings.LockedColor

end
end
end)

C.InputBeganConnection=u.InputBegan:Connect(function(E)
if not A then
l(function()
if E.KeyCode==Enum.KeyCode[s.Settings.TriggerKey]then
if s.Settings.Toggle then
B=not B

if not B then
CancelLock()
end
else
B=true
end
end
end)

l(function()
if E.UserInputType==Enum.UserInputType[s.Settings.TriggerKey]then
if s.Settings.Toggle then
B=not B

if not B then
CancelLock()
end
else
B=true
end
end
end)
end
end)

C.InputEndedConnection=u.InputEnded:Connect(function(E)
if not A then
if not s.Settings.Toggle then
l(function()
if E.KeyCode==Enum.KeyCode[s.Settings.TriggerKey]then
B=false;CancelLock()
end
end)

l(function()
if E.UserInputType==Enum.UserInputType[s.Settings.TriggerKey]then
B=false;CancelLock()
end
end)
end
end
end)
end



s.Functions={}

function s.Functions.Exit(E)
for F,G in n,C do
G:Disconnect()
end

if s.FOVCircle.Remove then s.FOVCircle:Remove()end

m().Aimbot.Functions=nil
m().Aimbot=nil

Load=nil;GetClosestPlayer=nil;CancelLock=nil
end

function s.Functions.Restart(E)
for F,G in n,C do
G:Disconnect()
end

Load()
end

function s.Functions.ResetSettings(E)
s.Settings={
Enabled=true,
TeamCheck=false,
AliveCheck=true,
WallCheck=false,
Sensitivity=0,
ThirdPerson=false,
ThirdPersonSensitivity=3,
TriggerKey="MouseButton2",
Toggle=false,
LockPart="Head"
}

s.FOVSettings={
Enabled=true,
Visible=true,
Amount=480,
Color=Color3.fromRGB(255,255,255),
LockedColor=Color3.fromRGB(255,70,70),
Transparency=0.5,
Sides=60,
Thickness=1,
Filled=false
}
end



Load()
end

if j==false then
pcall(function()
pcall(function()
getgenv().Aimbot.Functions:Exit()
end)
end)
end
end
})

Toggles.NameTags:OnChanged(function()
if Toggles.NameTags.Value==true then
for j,k in ipairs(game.Players:GetPlayers())do
if k.UserId~=b.UserId then
local l=Instance.new"BillboardGui"
l.Name="NameTag"
l.Size=UDim2.new(6.25,0,2.5,0)
l.AlwaysOnTop=true

local m=Instance.new"TextLabel"
m.Name="NameTag"
m.AnchorPoint=Vector2.new(0.5,0.5)
m.Position=UDim2.new(0.5,0,0.5,0)
m.Size=UDim2.new(1,0,1,0)
m.TextColor3=Color3.fromRGB(255,255,255)
m.Text=k.DisplayName.." (@"..k.Name..")"
m.BackgroundTransparency=1
m.TextScaled=true
m.RichText=true

local n=Instance.new"UIStroke"
n.Name="UIStroke"
n.ApplyStrokeMode=Enum.ApplyStrokeMode.Contextual
n.Color=Color3.fromRGB(0,0,0)
n.LineJoinMode=Enum.LineJoinMode.Miter
n.Thickness=1
n.Transparency=0

n.Parent=m
m.Parent=l
l.Parent=k.Character.Head
end
end
else
for j,k in ipairs(game.Players:GetPlayers())do
if k.Character.Head:FindFirstChild"NameTag"then
k.Character.Head:FindFirstChild"NameTag":Destroy()
end
end
end
end)

Toggles.ESP:OnChanged(function()
if Toggles.ESP.Value==true then
for j,k in ipairs(a:GetPlayers())do
if k.UserId~=b.UserId then

local l=Instance.new"BillboardGui"
l.Name="ESP"
l.Size=UDim2.new(5,0,6.5,0)
l.AlwaysOnTop=true

local m=Instance.new"Frame"
m.AnchorPoint=Vector2.new(0.5,0.5)
m.BackgroundColor3=Color3.fromRGB(255,0,4)
m.BackgroundTransparency=0.7
m.BorderSizePixel=0
m.Size=UDim2.new(0.9,0,0.9,0)
m.Position=UDim2.new(0.5,0,0.5,0)

local n=Instance.new"UIStroke"
n.ApplyStrokeMode=Enum.ApplyStrokeMode.Border
n.Color=Color3.fromRGB(255,255,255)
n.LineJoinMode=Enum.LineJoinMode.Miter
n.Thickness=1
n.Transparency=0

n.Parent=m
m.Parent=l
l.Parent=k.Character:FindFirstChild"HumanoidRootPart"

end
end
else
for j,k in ipairs(game.Players:GetPlayers())do
if k.Character.HumanoidRootPart:FindFirstChild"ESP"then
k.Character.HumanoidRootPart:FindFirstChild"ESP":Destroy()
end
end
end
end)



local j=g.Settings:AddLeftGroupbox'Menu'
local k=g.Settings:AddRightGroupbox'Debug'
local l=g.Settings:AddRightGroupbox'Debug'

k:AddButton('Infinite Yield',function()LoadInfyield()end)
k:AddButton('Load Debug UI',function()LoadDebugUI()end)
k:AddButton('Unload Debug UI',function()UnloadDebugUI()end)
l:AddButton('Load Dex',function()LoadDex()end)
l:AddButton('Kill Roblox',function()KillRoblox()end)
l:AddButton('Rejoin Game',function()RejoinSameGame()end)

j:AddButton('Unload',function()e:Unload()end)
j:AddLabel'Menu bind':AddKeyPicker('MenuKeybind',{Default='End',NoUI=true,Text='Menu keybind'})
