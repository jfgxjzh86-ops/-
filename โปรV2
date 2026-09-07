local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local UserInputService = game:GetService("UserInputService")
local Workspace = workspace

_G.SilentAimEnabled = true  
_G.WalkSpeed = 16           
_G.AimPart = "Head"         
_G.FOV_Radius = 150         
_G.FOV_Visible = true       
_G.TracerEnabled = true     
_G.EspEnabled = true        
_G.TeamCheck = true         
_G.WallCheck = true         
_G.MaxDistance = 300        

local currentTarget = nil

pcall(function()
    if CoreGui:FindFirstChild("StarkKeySystem") then CoreGui["StarkKeySystem"]:Destroy() end
    if CoreGui:FindFirstChild("SilentAimUI") then CoreGui["SilentAimUI"]:Destroy() end
    if LocalPlayer.PlayerGui:FindFirstChild("StarkKeySystem") then LocalPlayer.PlayerGui["StarkKeySystem"]:Destroy() end
    if LocalPlayer.PlayerGui:FindFirstChild("SilentAimUI") then LocalPlayer.PlayerGui["SilentAimUI"]:Destroy() end
end)

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SilentAimUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true

local successMainGui = pcall(function()
    ScreenGui.Parent = CoreGui
end)
if not successMainGui then
    ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
end

local Sides = 16
local fovLines = {}
pcall(function()
    for i = 1, Sides do
        local line = Drawing.new("Line")
        line.Visible = _G.FOV_Visible
        line.Thickness = 2
        line.Transparency = 1
        table.insert(fovLines, line)
    end
end)

local HeadSides = 12
local headRingLines = {}
pcall(function()
    for i = 1, HeadSides do
        local line = Drawing.new("Line")
        line.Visible = false
        line.Thickness = 2
        line.Transparency = 1
        table.insert(headRingLines, line)
    end
end)

local TracerLine
pcall(function()
    TracerLine = Drawing.new("Line")
    TracerLine.Visible = false
    TracerLine.Thickness = 1.5
    TracerLine.Transparency = 1
end)

local ToggleMenuBtn = Instance.new("TextButton")
ToggleMenuBtn.Size = UDim2.new(0, 120, 0, 40)
ToggleMenuBtn.Position = UDim2.new(0, 25, 0, 80)
ToggleMenuBtn.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
ToggleMenuBtn.Text = "SILENT AIM"
ToggleMenuBtn.TextColor3 = Color3.fromRGB(240, 240, 245)
ToggleMenuBtn.TextSize = 12
ToggleMenuBtn.Font = Enum.Font.GothamBold
ToggleMenuBtn.Active = true
ToggleMenuBtn.Draggable = true
ToggleMenuBtn.Parent = ScreenGui

local BtnCorner = Instance.new("UICorner")
BtnCorner.CornerRadius = UDim.new(0, 8)
BtnCorner.Parent = ToggleMenuBtn

local BtnStroke = Instance.new("UIStroke")
BtnStroke.Thickness = 1.5 
BtnStroke.Parent = ToggleMenuBtn

local BtnStrokeGradient = Instance.new("UIGradient")
BtnStrokeGradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 255, 255)), 
    ColorSequenceKeypoint.new(1, Color3.fromRGB(30, 30, 30))   
})
BtnStrokeGradient.Rotation = 90
BtnStrokeGradient.Parent = BtnStroke

local MenuFrame = Instance.new("Frame")
MenuFrame.Size = UDim2.new(0, 500, 0, 275)
MenuFrame.Position = UDim2.new(0, 25, 0, 130)
MenuFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
MenuFrame.Visible = true
MenuFrame.Active = true
MenuFrame.Draggable = true
MenuFrame.Parent = ScreenGui

local MenuCorner = Instance.new("UICorner")
MenuCorner.CornerRadius = UDim.new(0, 10)
MenuCorner.Parent = MenuFrame

local MenuStroke = Instance.new("UIStroke")
MenuStroke.Thickness = 1.5 
MenuStroke.Parent = MenuFrame

local MenuStrokeGradient = Instance.new("UIGradient")
MenuStrokeGradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 255, 255)), 
    ColorSequenceKeypoint.new(1, Color3.fromRGB(25, 25, 25))   
})
MenuStrokeGradient.Rotation = 90
MenuStrokeGradient.Parent = MenuStroke

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 24)
Title.Position = UDim2.new(0, 0, 0, 8)
Title.BackgroundTransparency = 1
Title.Text = "SILENT AIM"
Title.TextColor3 = Color3.fromRGB(235, 238, 245)
Title.TextSize = 11
Title.Font = Enum.Font.GothamBold
Title.Parent = MenuFrame

local CreatorLabel = Instance.new("TextLabel")
CreatorLabel.Size = UDim2.new(1, 0, 0, 14)
CreatorLabel.Position = UDim2.new(0, 0, 0, 26)
CreatorLabel.BackgroundTransparency = 1
CreatorLabel.Text = "By Stark Hub"
CreatorLabel.TextColor3 = Color3.fromRGB(140, 145, 160)
CreatorLabel.TextSize = 9
CreatorLabel.Font = Enum.Font.Gotham
CreatorLabel.Parent = MenuFrame

local function addHorizontalButton(text, startState, posX, posY, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 110, 0, 28)
    btn.Position = UDim2.new(0, posX, 0, posY)

    btn.BackgroundColor3 = startState and Color3.fromRGB(35, 15, 55) or Color3.fromRGB(18, 18, 22)
    btn.Text = text .. " : " .. (startState and "ON" or "OFF")
    btn.TextColor3 = startState and Color3.fromRGB(225, 180, 255) or Color3.fromRGB(160, 165, 180)
    btn.TextSize = 9
    btn.Font = Enum.Font.GothamBold
    btn.Parent = MenuFrame
    
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, 6)
    c.Parent = btn

    local stroke = Instance.new("UIStroke")
    stroke.Thickness = 1
    stroke.Color = startState and Color3.fromRGB(130, 50, 200) or Color3.fromRGB(50, 50, 60)
    stroke.Parent = btn
    
    local state = startState
    btn.MouseButton1Click:Connect(function()
        state = not state
        btn.BackgroundColor3 = state and Color3.fromRGB(35, 15, 55) or Color3.fromRGB(18, 18, 22)
        btn.TextColor3 = state and Color3.fromRGB(225, 180, 255) or Color3.fromRGB(160, 165, 180)
        stroke.Color = state and Color3.fromRGB(130, 50, 200) or Color3.fromRGB(50, 50, 60)
        btn.Text = text .. " : " .. (state and "ON" or "OFF")
        callback(state)
    end)
    return btn
end

addHorizontalButton("Silent Aim", _G.SilentAimEnabled, 16, 50, function(v) _G.SilentAimEnabled = v end)
addHorizontalButton("Team Check", _G.TeamCheck, 134, 50, function(v) _G.TeamCheck = v end)
addHorizontalButton("Wall Check", _G.WallCheck, 252, 50, function(v) _G.WallCheck = v end)

local AimPartBtn = Instance.new("TextButton")
AimPartBtn.Size = UDim2.new(0, 110, 0, 28)
AimPartBtn.Position = UDim2.new(0, 370, 0, 50)
AimPartBtn.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
AimPartBtn.Text = "Aim: Head"
AimPartBtn.TextColor3 = Color3.fromRGB(160, 165, 180)
AimPartBtn.TextSize = 9
AimPartBtn.Font = Enum.Font.GothamBold
AimPartBtn.Parent = MenuFrame

local AimPartCorner = Instance.new("UICorner")
AimPartCorner.CornerRadius = UDim.new(0, 6)
AimPartCorner.Parent = AimPartBtn

local AimPartStroke = Instance.new("UIStroke")
AimPartStroke.Thickness = 1
AimPartStroke.Color = Color3.fromRGB(50, 50, 60)
AimPartStroke.Parent = AimPartBtn

AimPartBtn.MouseButton1Click:Connect(function()
    if _G.AimPart == "Head" then
        _G.AimPart = "HumanoidRootPart"
        AimPartBtn.Text = "Aim: Body"
        AimPartBtn.BackgroundColor3 = Color3.fromRGB(35, 15, 55)
        AimPartBtn.TextColor3 = Color3.fromRGB(225, 180, 255)
        AimPartStroke.Color = Color3.fromRGB(130, 50, 200)
    else
        _G.AimPart = "Head"
        AimPartBtn.Text = "Aim: Head"
        AimPartBtn.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
        AimPartBtn.TextColor3 = Color3.fromRGB(160, 165, 180)
        AimPartStroke.Color = Color3.fromRGB(50, 50, 60)
    end
end)

addHorizontalButton("Show FOV", _G.FOV_Visible, 16, 86, function(v) _G.FOV_Visible = v end)
addHorizontalButton("Tracer Line", _G.TracerEnabled, 134, 86, function(v) _G.TracerEnabled = v end)
addHorizontalButton("ESP Highlight", _G.EspEnabled, 252, 86, function(v) _G.EspEnabled = v end)

local function createSlider(posY, labelText, defaultVal, minVal, maxVal, unit, callback)
    local container = Instance.new("Frame")
    container.Size = UDim2.new(0, 464, 0, 36)
    container.Position = UDim2.new(0, 16, 0, posY)
    container.BackgroundTransparency = 1
    container.Parent = MenuFrame

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 0, 16)
    label.BackgroundTransparency = 1
    label.Text = labelText .. " : " .. tostring(defaultVal) .. (unit or "")
    label.TextColor3 = Color3.fromRGB(190, 195, 210)
    label.TextSize = 10
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = container

    local track = Instance.new("Frame")
    track.Size = UDim2.new(1, 0, 0, 6)
    track.Position = UDim2.new(0, 0, 0, 20)
    track.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
    track.BorderSizePixel = 0
    track.Parent = container

    local trackCorner = Instance.new("UICorner")
    trackCorner.CornerRadius = UDim.new(1, 0)
    trackCorner.Parent = track

    local trackStroke = Instance.new("UIStroke")
    trackStroke.Thickness = 1
    trackStroke.Color = Color3.fromRGB(50, 50, 60)
    trackStroke.Parent = track

    local initialPer = math.clamp((defaultVal - minVal) / (maxVal - minVal), 0, 1)

    local fill = Instance.new("Frame")
    fill.Size = UDim2.new(initialPer, 0, 1, 0)
    fill.BackgroundColor3 = Color3.fromRGB(180, 180, 190)
    fill.BorderSizePixel = 0
    fill.Parent = track

    local fillCorner = Instance.new("UICorner")
    fillCorner.CornerRadius = UDim.new(1, 0)
    fillCorner.Parent = fill

    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 12, 0, 12)
    btn.AnchorPoint = Vector2.new(0.5, 0.5)
    btn.Position = UDim2.new(initialPer, 0, 0.5, 0)
    btn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    btn.Text = ""
    btn.Parent = track

    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(1, 0)
    btnCorner.Parent = btn

    local sliding = false
    local function update(input)
        local trackWidth = track.AbsoluteSize.X
        if trackWidth <= 0 then return end
        local relativeX = input.Position.X - track.AbsolutePosition.X
        local percentage = math.clamp(relativeX / trackWidth, 0, 1)
        local value = math.round(minVal + (percentage * (maxVal - minVal)))
        label.Text = labelText .. " : " .. tostring(value) .. (unit or "")
        fill.Size = UDim2.new(percentage, 0, 1, 0)
        btn.Position = UDim2.new(percentage, 0, 0.5, 0)
        callback(value)
    end

    btn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then sliding = true end
    end)
    track.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            sliding = true
            update(input)
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if sliding and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            update(input)
        end
    end)
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            sliding = false
        end
    end)
end

createSlider(130, "FOV Size", _G.FOV_Radius, 20, 400, "", function(v) _G.FOV_Radius = v end)
createSlider(172, "WalkSpeed", _G.WalkSpeed, 16, 200, "", function(v) _G.WalkSpeed = v end)
createSlider(214, "Max Distance", _G.MaxDistance, 50, 1000, " Studs", function(v) _G.MaxDistance = v end)

ToggleMenuBtn.MouseButton1Click:Connect(function()
    MenuFrame.Visible = not MenuFrame.Visible
end)

local function isTeammate(player)
    if not _G.TeamCheck then return false end
    if player.Team and LocalPlayer.Team and player.Team == LocalPlayer.Team then return true end
    return false
end

local function isVisible(targetPart)
    if not _G.WallCheck then return true end
    if not LocalPlayer.Character or not LocalPlayer.Character:FindFirstChild("Head") then return true end
    local origin = LocalPlayer.Character.Head.Position
    local direction = targetPart.Position - origin
    local raycastParams = RaycastParams.new()
    raycastParams.FilterType = Enum.RaycastFilterType.Exclude
    local ignoreList = {LocalPlayer.Character}
    if targetPart.Parent then table.insert(ignoreList, targetPart.Parent) end
    raycastParams.FilterDescendantsInstances = ignoreList
    raycastParams.IgnoreWater = true
    local raycastResult = Workspace:Raycast(origin, direction, raycastParams)
    if raycastResult then return false end
    return true
end

local function getSilentTarget()
    if not _G.SilentAimEnabled then return nil end
    local dist = math.huge
    local closest = nil
    local center = Camera.ViewportSize / 2
    local localRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and not isTeammate(v) then
            local humanoid = v.Character:FindFirstChildOfClass("Humanoid")
            if humanoid and humanoid.Health > 0 then
                local part = v.Character:FindFirstChild(_G.AimPart) or v.Character:FindFirstChild("Head")
                if part and localRoot then
                    local distToPlayer = (part.Position - localRoot.Position).Magnitude
                    if distToPlayer <= _G.MaxDistance then
                        local pos, vis = Camera:WorldToViewportPoint(part.Position)
                        if vis and isVisible(part) then
                            local screenPos = Vector2.new(pos.X, pos.Y)
                            local mag = (screenPos - center).Magnitude
                            if mag <= _G.FOV_Radius and mag < dist then
                                dist = mag
                                closest = part
                            end
                        end
                    end
                end
            end
        end
    end
    return closest
end

pcall(function()
    local oldnc
    oldnc = hookmetamethod(game, "__namecall", function(self, ...)
        local method = getnamecallmethod()
        local args = {...}
        if _G.SilentAimEnabled and currentTarget and self == Workspace then
            if method == "Raycast" and args[1] and args[2] then
                args[2] = (currentTarget.Position - args[1]).Unit * args[2].Magnitude
            elseif method == "FindPartOnRay" and args[1] then
                args[1] = Ray.new(args[1].Origin, (currentTarget.Position - args[1].Origin).Unit * args[1].Direction.Magnitude)
            end
        end
        return oldnc(self, unpack(args))
    end)
end)

local function applyESP(player)
    if player == LocalPlayer then return end
    local function setupChar(char)
        if char:FindFirstChild("ProHighlight") then return end
        local h = Instance.new("Highlight")
        h.Name = "ProHighlight"
        h.Adornee = char
        h.FillTransparency = 0.5
        h.FillColor = Color3.fromRGB(255, 0, 0)
        h.OutlineColor = Color3.fromRGB(255, 255, 255)
        h.Parent = char
    end
    if player.Character then setupChar(player.Character) end
    player.CharacterAdded:Connect(setupChar)
end

for _, p in pairs(Players:GetPlayers()) do applyESP(p) end
Players.PlayerAdded:Connect(applyESP)

local hue = 0
local ringAngle = 0

RunService.RenderStepped:Connect(function()
    pcall(function()
        currentTarget = getSilentTarget()
        
        -- FOV Logic
        hue = (hue + 0.5 / 100) % 1
        local center = Camera.ViewportSize / 2
        for i = 1, Sides do
            local a1 = ((i - 1) / Sides) * math.pi * 2
            local a2 = (i / Sides) * math.pi * 2

            local p1 = center + Vector2.new(
                math.cos(a1) * _G.FOV_Radius,
                math.sin(a1) * _G.FOV_Radius
            )

            local p2 = center + Vector2.new(
                math.cos(a2) * _G.FOV_Radius,
                math.sin(a2) * _G.FOV_Radius
            )

            local line = fovLines[i]
            if line then
                line.Visible = _G.FOV_Visible
                line.From = p1
                line.To = p2
                line.Color = Color3.fromHSV((hue + i / Sides) % 1, 1, 1)
            end
        end

        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
            LocalPlayer.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = _G.WalkSpeed
        end

        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character then
                local highlight = p.Character:FindFirstChild("ProHighlight")
                if highlight then
                    if _G.EspEnabled and not isTeammate(p) then
                        highlight.Enabled = true
                        highlight.FillColor = Color3.fromRGB(255, 0, 0)
                    else
                        highlight.Enabled = false
                    end
                end
            end
        end

        local activeTarget = currentTarget

-- Rotating Ring Logic
        if activeTarget and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head") then
            ringAngle = ringAngle + 0.15
            local headPart = activeTarget.Parent:FindFirstChild("Head") or activeTarget
            local headPos = headPart.Position
            local radius = 1.15
            
            local lookVector = (LocalPlayer.Character.Head.Position - headPos).Unit
            local rightVector = lookVector:Cross(Vector3.new(0, 1, 0))
            if rightVector.Magnitude == 0 then rightVector = Vector3.new(1, 0, 0) else rightVector = rightVector.Unit end
            local upVector = rightVector:Cross(lookVector).Unit

            for i = 1, HeadSides do
                local a1 = ((i - 1) / HeadSides) * math.pi * 2 + ringAngle
                local a2 = (i / HeadSides) * math.pi * 2 + ringAngle
                
                local p3D1 = headPos + (rightVector * math.cos(a1) + upVector * math.sin(a1)) * radius + (lookVector * 0.5)
                local p3D2 = headPos + (rightVector * math.cos(a2) + upVector * math.sin(a2)) * radius + (lookVector * 0.5)
                
                local screen1, vis1 = Camera:WorldToViewportPoint(p3D1)
                local screen2, vis2 = Camera:WorldToViewportPoint(p3D2)
                
                local rLine = headRingLines[i]
                if rLine then
                    if vis1 and vis2 then
                        rLine.Visible = true
                        rLine.From = Vector2.new(screen1.X, screen1.Y)
                        rLine.To = Vector2.new(screen2.X, screen2.Y)
                        rLine.Color = Color3.fromHSV(hue, 1, 1)
                    else
                        rLine.Visible = false
                    end
                end
            end
        else
            for _, rLine in pairs(headRingLines) do
                if rLine then rLine.Visible = false end
            end
        end

        if _G.TracerEnabled and activeTarget then
            local pos, vis = Camera:WorldToViewportPoint(activeTarget.Position)
            if vis then
                TracerLine.Visible = true
                TracerLine.From = center
                TracerLine.To = Vector2.new(pos.X, pos.Y)
                TracerLine.Color = Color3.fromHSV(hue, 1, 1)
            else
                TracerLine.Visible = false
            end
        else
            if TracerLine then TracerLine.Visible = false end
        end
    end)
end)
-- วางไว้ใน StarterPlayerScripts (LocalScript)
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- 1. ระบบกวาดล้างและเฝ้าระวัง GUI แปลกปลอม (เช่น เมนู Silent Aim, Hub, Cheat)
local function inspectAndDestroy(instance)
    if instance:IsA("ScreenGui") then
        local nameLower = instance.Name:lower()
        -- รายชื่อคำที่มักใช้ตั้งชื่อโปรแกรมโกงหรือเมนู
        local blacklistKeywords = {"silent", "aim", "hub", "cheat", "stark", "esp", "menu", "gui", "exploit"}
        
        for _, keyword in ipairs(blacklistKeywords) do
            if nameLower:find(keyword) and instance.Name ~= "RobloxGui" then
                instance:Destroy()
                
                -- ส่งสัญญาณเตือนไปที่ Server (ถ้ามี RemoteEvent เตรียมไว้)
                local antiCheatEvent = ReplicatedStorage:FindFirstChild("AntiCheatAlert")
                if antiCheatEvent then
                    antiCheatEvent:FireServer("Detected unauthorized UI: " .. instance.Name)
                end
                break
            end
        end
    end
end

