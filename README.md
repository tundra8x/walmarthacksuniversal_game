-- Services

local Players = game:GetService("Players")

-- Variables

local Player = Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()

-- Built-in Functions

local function LoadInfyield()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
end

local function LoadDebugUI()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/tundra8x/walmarthacksuniversal/refs/heads/main/README.md"))();
end

local function LoadDex()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/infyiff/backup/main/dex.lua"))()
end

local function UnloadDebugUI()
	Player.PlayerGui.Debug:Destroy()
end

local function KillRoblox()
	game:Shutdown()
end

local function RejoinSameGame()
    local teleportService = game:GetService("TeleportService")
    local placeId = game.PlaceId
    local serverId = game.JobId
    teleportService:TeleportToPlaceInstance(placeId, serverId, Player)
end

local function LoadFunkyFridayScript()
	Library:Unload()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/Nadir3709/RandomScript/main/FunkyFridayMobile"))()
end

local function LoadFischScript()
	Library:Unload()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/Efe0626/RaitoHub/main/Script"))()
end

local function LoadBloxFruitsScript()
	Library:Unload()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/realredz/BloxFruits/refs/heads/main/Source.lua"))()
end

local function LoadDesSimulatorScript()
	Library:Unload()
	loadstring(game:HttpGet("https://waza-scripts.vercel.app/script/DestructionSimulator"))()
end

-- Repository & Libraries

local repo = 'https://raw.githubusercontent.com/violin-suzutsuki/LinoriaLib/main/'

local Library = loadstring(game:HttpGet(repo .. 'Library.lua'))()
local ThemeManager = loadstring(game:HttpGet(repo .. 'addons/ThemeManager.lua'))()
local SaveManager = loadstring(game:HttpGet(repo .. 'addons/SaveManager.lua'))()

-- Windows

local hacks = Library:CreateWindow({
	Title = 'walmart hacks lul v0.1| universal | non-backdoor',
	Center = true,
	AutoShow = true,
	TabPadding = 8,
	MenuFadeTime = 0.2
})

-- Tabs

local Tabs = {
	Home = hacks:AddTab('Home'),
	Visuals = hacks:AddTab('Visuals'),
	Blatant = hacks:AddTab('Blatant'),
	Settings = hacks:AddTab('Settings'),
	Other = hacks:AddTab('Other Scripts'),
}

-- Global Variables

_G.ESP = false
_G.NameTags = false
_G.Aimbot = false
_G.SoftAim = false

-- Groups

local VisualsGroup = Tabs.Visuals:AddLeftGroupbox('Visuals')
local BlatantGroup = Tabs.Blatant:AddLeftGroupbox('Blatant')

-- Toggles

-- Visuals Group

VisualsGroup:AddToggle('ESP', {
	Text = 'ESP',
	Default = false,
	Tooltip = 'Allows you to see people through walls',

	Callback = function(espVal)
		_G.ESP = espVal
	end
})

VisualsGroup:AddToggle('NameTags', {
	Text = 'NameTags',
	Default = false,
	Tooltip = 'Allows you to see peoples usernames through walls',

	Callback = function(nameTagsVal)	
		_G.NameTags = nameTagsVal
	end
})

-- Blatant Group

BlatantGroup:AddToggle('Aimbot', {
	Text = 'Aimbot',
	Default = false,
	Tooltip = 'Locks on to the closest players head',

	Callback = function(aimbotVal)
		if aimbotVal == true then
			_G.Aimbot = true
		end
		
		while wait() do
			
			if _G.Aimbot == false then return end
			
			--// Cache

			local select = select
			local pcall, getgenv, next, Vector2, mathclamp, type, mousemoverel = select(1, pcall, getgenv, next, Vector2.new, math.clamp, type, mousemoverel or (Input and Input.MouseMove))

			--// Preventing Multiple Processes

			pcall(function()
				getgenv().Aimbot.Functions:Exit()
			end)

			--// Environment

			getgenv().Aimbot = {}
			local Environment = getgenv().Aimbot

			--// Services

			local RunService = game:GetService("RunService")
			local UserInputService = game:GetService("UserInputService")
			local TweenService = game:GetService("TweenService")
			local Players = game:GetService("Players")
			local Camera = workspace.CurrentCamera
			local LocalPlayer = Players.LocalPlayer

			--// Variables

			local RequiredDistance, Typing, Running, Animation, ServiceConnections = 2000, false, false, nil, {}

			--// Script Settings

			Environment.Settings = {
				Enabled = true,
				TeamCheck = false,
				AliveCheck = true,
				WallCheck = false, -- Laggy
				Sensitivity = 0, -- Animation length (in seconds) before fully locking onto target
				ThirdPerson = false, -- Uses mousemoverel instead of CFrame to support locking in third person (could be choppy)
				ThirdPersonSensitivity = 3, -- Boundary: 0.1 - 5
				TriggerKey = "MouseButton2",
				Toggle = false,
				LockPart = "Head" -- Body part to lock on
			}

			Environment.FOVSettings = {
				Enabled = true,
				Visible = true,
				Amount = 480,
				Color = Color3.fromRGB(255, 255, 255),
				LockedColor = Color3.fromRGB(255, 70, 70),
				Transparency = 0.5,
				Sides = 60,
				Thickness = 1,
				Filled = false
			}

			Environment.FOVCircle = Drawing.new("Circle")

			--// Functions

			local function CancelLock()
				Environment.Locked = nil
				if Animation then Animation:Cancel() end
				Environment.FOVCircle.Color = Environment.FOVSettings.Color
			end

			local function GetClosestPlayer()
				if not Environment.Locked then
					RequiredDistance = (Environment.FOVSettings.Enabled and Environment.FOVSettings.Amount or 2000)

					for _, v in next, Players:GetPlayers() do
						if v ~= LocalPlayer then
							if v.Character and v.Character:FindFirstChild(Environment.Settings.LockPart) and v.Character:FindFirstChildOfClass("Humanoid") then
								if Environment.Settings.TeamCheck and v.Team == LocalPlayer.Team then continue end
								if Environment.Settings.AliveCheck and v.Character:FindFirstChildOfClass("Humanoid").Health <= 0 then continue end
								if Environment.Settings.WallCheck and #(Camera:GetPartsObscuringTarget({v.Character[Environment.Settings.LockPart].Position}, v.Character:GetDescendants())) > 0 then continue end

								local Vector, OnScreen = Camera:WorldToViewportPoint(v.Character[Environment.Settings.LockPart].Position)
								local Distance = (Vector2(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2(Vector.X, Vector.Y)).Magnitude

								if Distance < RequiredDistance and OnScreen then
									RequiredDistance = Distance
									Environment.Locked = v
								end
							end
						end
					end
				elseif (Vector2(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2(Camera:WorldToViewportPoint(Environment.Locked.Character[Environment.Settings.LockPart].Position).X, Camera:WorldToViewportPoint(Environment.Locked.Character[Environment.Settings.LockPart].Position).Y)).Magnitude > RequiredDistance then
					CancelLock()
				end
			end

			--// Typing Check

			ServiceConnections.TypingStartedConnection = UserInputService.TextBoxFocused:Connect(function()
				Typing = true
			end)

			ServiceConnections.TypingEndedConnection = UserInputService.TextBoxFocusReleased:Connect(function()
				Typing = false
			end)

			--// Main

			local function Load()
				ServiceConnections.RenderSteppedConnection = RunService.RenderStepped:Connect(function()
					if Environment.FOVSettings.Enabled and Environment.Settings.Enabled then
						Environment.FOVCircle.Radius = Environment.FOVSettings.Amount
						Environment.FOVCircle.Thickness = Environment.FOVSettings.Thickness
						Environment.FOVCircle.Filled = Environment.FOVSettings.Filled
						Environment.FOVCircle.NumSides = Environment.FOVSettings.Sides
						Environment.FOVCircle.Color = Environment.FOVSettings.Color
						Environment.FOVCircle.Transparency = Environment.FOVSettings.Transparency
						Environment.FOVCircle.Visible = Environment.FOVSettings.Visible
						Environment.FOVCircle.Position = Vector2(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y)
					else
						Environment.FOVCircle.Visible = false
					end

					if Running and Environment.Settings.Enabled then
						GetClosestPlayer()

						if Environment.Locked then
							if Environment.Settings.ThirdPerson then
								Environment.Settings.ThirdPersonSensitivity = mathclamp(Environment.Settings.ThirdPersonSensitivity, 0.1, 5)

								local Vector = Camera:WorldToViewportPoint(Environment.Locked.Character[Environment.Settings.LockPart].Position)
								mousemoverel((Vector.X - UserInputService:GetMouseLocation().X) * Environment.Settings.ThirdPersonSensitivity, (Vector.Y - UserInputService:GetMouseLocation().Y) * Environment.Settings.ThirdPersonSensitivity)
							else
								if Environment.Settings.Sensitivity > 0 then
									Animation = TweenService:Create(Camera, TweenInfo.new(Environment.Settings.Sensitivity, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {CFrame = CFrame.new(Camera.CFrame.Position, Environment.Locked.Character[Environment.Settings.LockPart].Position)})
									Animation:Play()
								else
									Camera.CFrame = CFrame.new(Camera.CFrame.Position, Environment.Locked.Character[Environment.Settings.LockPart].Position)
								end
							end

							Environment.FOVCircle.Color = Environment.FOVSettings.LockedColor

						end
					end
				end)

				ServiceConnections.InputBeganConnection = UserInputService.InputBegan:Connect(function(Input)
					if not Typing then
						pcall(function()
							if Input.KeyCode == Enum.KeyCode[Environment.Settings.TriggerKey] then
								if Environment.Settings.Toggle then
									Running = not Running

									if not Running then
										CancelLock()
									end
								else
									Running = true
								end
							end
						end)

						pcall(function()
							if Input.UserInputType == Enum.UserInputType[Environment.Settings.TriggerKey] then
								if Environment.Settings.Toggle then
									Running = not Running

									if not Running then
										CancelLock()
									end
								else
									Running = true
								end
							end
						end)
					end
				end)

				ServiceConnections.InputEndedConnection = UserInputService.InputEnded:Connect(function(Input)
					if not Typing then
						if not Environment.Settings.Toggle then
							pcall(function()
								if Input.KeyCode == Enum.KeyCode[Environment.Settings.TriggerKey] then
									Running = false; CancelLock()
								end
							end)

							pcall(function()
								if Input.UserInputType == Enum.UserInputType[Environment.Settings.TriggerKey] then
									Running = false; CancelLock()
								end
							end)
						end
					end
				end)
			end

			--// Functions

			Environment.Functions = {}

			function Environment.Functions:Exit()
				for _, v in next, ServiceConnections do
					v:Disconnect()
				end

				if Environment.FOVCircle.Remove then Environment.FOVCircle:Remove() end

				getgenv().Aimbot.Functions = nil
				getgenv().Aimbot = nil

				Load = nil; GetClosestPlayer = nil; CancelLock = nil
			end

			function Environment.Functions:Restart()
				for _, v in next, ServiceConnections do
					v:Disconnect()
				end

				Load()
			end

			function Environment.Functions:ResetSettings()
				Environment.Settings = {
					Enabled = true,
					TeamCheck = false,
					AliveCheck = true,
					WallCheck = false,
					Sensitivity = 0, -- Animation length (in seconds) before fully locking onto target
					ThirdPerson = false, -- Uses mousemoverel instead of CFrame to support locking in third person (could be choppy)
					ThirdPersonSensitivity = 3, -- Boundary: 0.1 - 5
					TriggerKey = "MouseButton2",
					Toggle = false,
					LockPart = "Head" -- Body part to lock on
				}

				Environment.FOVSettings = {
					Enabled = true,
					Visible = true,
					Amount = 480,
					Color = Color3.fromRGB(255, 255, 255),
					LockedColor = Color3.fromRGB(255, 70, 70),
					Transparency = 0.5,
					Sides = 60,
					Thickness = 1,
					Filled = false
				}
			end

			--// Load

			Load()
		end
		
		if aimbotVal == false then
			pcall(function()
				pcall(function()
					getgenv().Aimbot.Functions:Exit()
				end)
			end)
		end
	end
})

Toggles.NameTags:OnChanged(function()
	if Toggles.NameTags.Value == true then
		for _, plr in ipairs(game.Players:GetPlayers()) do
			if plr.UserId ~= Player.UserId then
				local billboard = Instance.new("BillboardGui")
				billboard.Name = "NameTag"
				billboard.Size = UDim2.new(6.25,0,2.5,0)
				billboard.AlwaysOnTop = true

				local nameTag = Instance.new("TextLabel")
				nameTag.Name = "NameTag"
				nameTag.AnchorPoint = Vector2.new(0.5,0.5)
				nameTag.Position = UDim2.new(0.5,0,0.5,0)
				nameTag.Size = UDim2.new(1,0,1,0)
				nameTag.TextColor3 = Color3.fromRGB(255,255,255)
				nameTag.Text = plr.DisplayName.." (@"..plr.Name..")"
				nameTag.BackgroundTransparency = 1
				nameTag.TextScaled = true
				nameTag.RichText = true
				
				local uistroke = Instance.new("UIStroke")
				uistroke.Name = "UIStroke"
				uistroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual
				uistroke.Color = Color3.fromRGB(0,0,0)
				uistroke.LineJoinMode = Enum.LineJoinMode.Miter
				uistroke.Thickness = 1
				uistroke.Transparency = 0
				
				uistroke.Parent = nameTag
				nameTag.Parent = billboard
				billboard.Parent = plr.Character.Head
			end
		end
	else
		for _, plr in ipairs(game.Players:GetPlayers()) do
			if plr.Character.Head:FindFirstChild("NameTag") then
				plr.Character.Head:FindFirstChild("NameTag"):Destroy()
			end
		end
	end
end)

Toggles.ESP:OnChanged(function()
	if Toggles.ESP.Value == true then
		for _, plr in ipairs(Players:GetPlayers()) do
			if plr.UserId ~= Player.UserId then

				local bbgui = Instance.new("BillboardGui")
				bbgui.Name = "ESP"
				bbgui.Size = UDim2.new(5,0,6.5,0)
				bbgui.AlwaysOnTop = true

				local frame = Instance.new("Frame")
				frame.AnchorPoint = Vector2.new(0.5,0.5)
				frame.BackgroundColor3 = Color3.fromRGB(255, 0, 4)
				frame.BackgroundTransparency = 0.7
				frame.BorderSizePixel = 0
				frame.Size = UDim2.new(0.9,0,0.9,0)
				frame.Position = UDim2.new(0.5,0,0.5,0)

				local stroke = Instance.new("UIStroke")
				stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
				stroke.Color = Color3.fromRGB(255, 255, 255)
				stroke.LineJoinMode = Enum.LineJoinMode.Miter
				stroke.Thickness = 1
				stroke.Transparency = 0

				stroke.Parent = frame
				frame.Parent = bbgui
				bbgui.Parent = plr.Character:FindFirstChild("HumanoidRootPart")

			end
		end
	else
		for _, plr in ipairs(game.Players:GetPlayers()) do
			if plr.Character.HumanoidRootPart:FindFirstChild("ESP") then
				plr.Character.HumanoidRootPart:FindFirstChild("ESP"):Destroy()
			end
		end
	end
end)

-- Settings

local MenuGroup = Tabs.Settings:AddLeftGroupbox('Menu')
local Debug = Tabs.Settings:AddRightGroupbox('Debug')
local Debug2 = Tabs.Settings:AddRightGroupbox('Debug')

local Scripts = Tabs.Other:AddLeftGroupbox('Scripts')
local Scripts2 = Tabs.Other:AddRightGroupbox('Scripts')

Debug:AddButton('Infinite Yield', function() LoadInfyield() end)
Debug:AddButton('Load Debug UI', function() LoadDebugUI() end)
Debug:AddButton('Unload Debug UI', function() UnloadDebugUI() end)
Debug2:AddButton('Load Dex', function() LoadDex() end)
Debug2:AddButton('Kill Roblox', function() KillRoblox() end)
Debug2:AddButton('Rejoin Game', function() RejoinSameGame() end)

Scripts:AddButton('Funky Friday', function() LoadFunkyFridayScript() end)
Scripts:AddButton('Fisch', function() LoadFischScript() end)
Scripts:AddButton('Blox Fruits', function() LoadBloxFruitsScript() end)

Scripts2:AddButton('Destruction Simulator', function() LoadDesSimulatorScript() end)

MenuGroup:AddButton('Unload', function() Library:Unload() end)
MenuGroup:AddLabel('Menu bind'):AddKeyPicker('MenuKeybind', { Default = 'End', NoUI = true, Text = 'Menu keybind' })
