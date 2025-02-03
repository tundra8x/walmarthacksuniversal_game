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

local function UnloadDebugUI()
	Player.PlayerGui.Debug.Enabled = false
end

-- Repository & Libraries

local repo = 'https://raw.githubusercontent.com/violin-suzutsuki/LinoriaLib/main/'

local Library = loadstring(game:HttpGet(repo .. 'Library.lua'))()
local ThemeManager = loadstring(game:HttpGet(repo .. 'addons/ThemeManager.lua'))()
local SaveManager = loadstring(game:HttpGet(repo .. 'addons/SaveManager.lua'))()

-- Windows

local hacks = Library:CreateWindow({
    Title = 'walmart hacks lul | universal',
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
        _G.Aimbot = aimbotVal
    end
})

BlatantGroup:AddToggle('Soft Aim', {
    Text = 'Soft Aim',
    Default = false,
    Tooltip = 'A less obvious version of aimbot',

    Callback = function(softAimVal)
        _G.SoftAim = softAimVal
    end
})

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
		
	end
end)

-- Settings

local MenuGroup = Tabs.Settings:AddLeftGroupbox('Menu')
local Debug = Tabs.Settings:AddRightGroupbox('Debug')

Debug:AddButton('Infinite Yield', function() LoadInfyield() end)
Debug:AddButton('Load Debug UI', function() LoadDebugUI() end)
Debug:AddButton('Unload Debug UI', function() UnloadDebugUI() end)

MenuGroup:AddButton('Unload', function() Library:Unload() end)
MenuGroup:AddLabel('Menu bind'):AddKeyPicker('MenuKeybind', { Default = 'End', NoUI = true, Text = 'Menu keybind' })
