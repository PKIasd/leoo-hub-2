--[[
Leoo Hub - Feito com amor
Autor: leoo
Descrição: Hub flutuante em preto com bola cinza. Clique na bola para abrir/fechar.
Opções em branco: pulos infinito, noclip, fly, velocidade 100, esp, visão no escuro, teleporte, godmode, no slow, créditos.
]]

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

-- Criar ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "LeooHub"
screenGui.Parent = game.CoreGui

-- Bola cinza
local ball = Instance.new("Frame")
ball.Size = UDim2.new(0, 60, 0, 60)
ball.Position = UDim2.new(0, 50, 0, 250)
ball.BackgroundColor3 = Color3.fromRGB(130,130,130)
ball.BorderSizePixel = 0
ball.AnchorPoint = Vector2.new(0.5,0.5)
ball.Name = "LeooBall"
ball.Parent = screenGui
ball.Active = true
ball.Draggable = true

local ballCorner = Instance.new("UICorner", ball)
ballCorner.CornerRadius = UDim.new(1,0)

-- Hub
local hub = Instance.new("Frame")
hub.Size = UDim2.new(0, 330, 0, 340)
hub.Position = UDim2.new(0, 120, 0, 180)
hub.BackgroundColor3 = Color3.new(0,0,0)
hub.BorderSizePixel = 0
hub.AnchorPoint = Vector2.new(0.5,0.5)
hub.Visible = false
hub.Active = true
hub.Draggable = true
hub.Parent = screenGui

local hubCorner = Instance.new("UICorner", hub)
hubCorner.CornerRadius = UDim.new(0.07,0)

-- Título
local title = Instance.new("TextLabel")
title.Parent = hub
title.Size = UDim2.new(1, 0, 0, 40)
title.Position = UDim2.new(0,0,0,0)
title.BackgroundTransparency = 1
title.Text = "leoo hub - feito com amor -"
title.TextColor3 = Color3.new(1,1,1)
title.Font = Enum.Font.GothamBold
title.TextSize = 22

-- Opções
local options = {
    "pulos infinito",
    "noclip",
    "fly",
    "velocidade 100",
    "esp",
    "visao no escuro",
    "teleporte",
    "godmode",
    "no slow",
    "creditos",
}
local optionFuncs = {}

local function setCredits()
    local creditFrame = Instance.new("Frame", hub)
    creditFrame.Size = UDim2.new(0.8,0,0,40)
    creditFrame.Position = UDim2.new(0.1,0,0.85,0)
    creditFrame.BackgroundTransparency = 0.3
    creditFrame.BackgroundColor3 = Color3.fromRGB(30,30,30)
    local corner = Instance.new("UICorner", creditFrame)
    corner.CornerRadius = UDim.new(1,0)
    local creditText = Instance.new("TextLabel", creditFrame)
    creditText.Size = UDim2.new(1,0,1,0)
    creditText.BackgroundTransparency = 1
    creditText.Text = "Feito por leoo <3"
    creditText.TextColor3 = Color3.new(1,1,1)
    creditText.Font = Enum.Font.GothamBold
    creditText.TextSize = 18
    delay(2, function()
        creditFrame:Destroy()
    end)
end

for i,v in ipairs(options) do
    local btn = Instance.new("TextButton")
    btn.Parent = hub
    btn.Size = UDim2.new(0.9,0,0,28)
    btn.Position = UDim2.new(0.05,0,0,45+(30*(i-1)))
    btn.BackgroundColor3 = Color3.fromRGB(20,20,20)
    btn.Text = v
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 17
    btn.Name = "Option"..i
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0.3,0)
    btn.MouseButton1Click:Connect(function()
        if v == "creditos" then
            setCredits()
        else
            if optionFuncs[v] then
                optionFuncs[v]()
            end
        end
    end)
end

-- Funções das opções
optionFuncs["pulos infinito"] = function()
    LocalPlayer.Character.Humanoid.JumpPower = 500
end

optionFuncs["noclip"] = function()
    local noclip = true
    RunService.Stepped:Connect(function()
        if noclip and LocalPlayer.Character then
            for _,v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.CanCollide = false
                end
            end
        end
    end)
end

optionFuncs["fly"] = function()
    local flying = true
    local speed = 2
    local function fly()
        local humanoidRootPart = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if not humanoidRootPart then return end
        RunService.RenderStepped:Connect(function()
            if flying then
                humanoidRootPart.Velocity = Vector3.new(0, speed*20, 0)
            end
        end)
    end
    fly()
end

optionFuncs["velocidade 100"] = function()
    LocalPlayer.Character.Humanoid.WalkSpeed = 100
end

optionFuncs["esp"] = function()
    for _,p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character then
            for _,v in pairs(p.Character:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.Color = Color3.fromRGB(130,130,130)
                end
            end
        end
    end
end

optionFuncs["visao no escuro"] = function()
    game.Lighting.Brightness = 3
    game.Lighting.ClockTime = 14
    game.Lighting.FogEnd = 100000
end

optionFuncs["teleporte"] = function()
    Mouse.KeyDown:Connect(function(key)
        if key:lower() == "y" then
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(Mouse.Hit.p)
            end
        end
    end)
end

optionFuncs["godmode"] = function()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.Health = math.huge
        LocalPlayer.Character.Humanoid.MaxHealth = math.huge
        LocalPlayer.Character.Humanoid:GetPropertyChangedSignal("Health"):Connect(function()
            LocalPlayer.Character.Humanoid.Health = math.huge
        end)
    end
end

optionFuncs["no slow"] = function()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
            LocalPlayer.Character.Humanoid.WalkSpeed = 16
        end)
    end
end

-- Abrir/Fechar hub ao clicar na bola
local hubOpen = false
ball.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        hubOpen = not hubOpen
        hub.Visible = hubOpen
    end
end)

-- Arrastar o hub e a bola
local dragFrame, dragInput, dragStart, startPos
local function dragify(frame)
    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragInput = input
            dragStart = input.Position
            startPos = frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragInput = nil
                end
            end)
        end
    end)
    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement and dragInput then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

dragify(ball)
dragify(hub)
