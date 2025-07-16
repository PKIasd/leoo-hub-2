-- Estados das opções
local optionStates = {}
for _, v in ipairs(options) do
    optionStates[v] = false
end

-- Funções das opções com toggle
optionFuncs["pulos infinito"] = function()
    optionStates["pulos infinito"] = not optionStates["pulos infinito"]
    if optionStates["pulos infinito"] then
        LocalPlayer.Character.Humanoid.JumpPower = 500
    else
        LocalPlayer.Character.Humanoid.JumpPower = 50
    end
end

optionFuncs["noclip"] = function()
    optionStates["noclip"] = not optionStates["noclip"]
    if optionStates["noclip"] then
        optionFuncs["noclip"].conn = RunService.Stepped:Connect(function()
            if optionStates["noclip"] and LocalPlayer.Character then
                for _,v in pairs(LocalPlayer.Character:GetDescendants()) do
                    if v:IsA("BasePart") then
                        v.CanCollide = false
                    end
                end
            end
        end)
    else
        if optionFuncs["noclip"].conn then optionFuncs["noclip"].conn:Disconnect() end
        for _,v in pairs(LocalPlayer.Character:GetDescendants()) do
            if v:IsA("BasePart") then
                v.CanCollide = true
            end
        end
    end
end

optionFuncs["fly"] = function()
    optionStates["fly"] = not optionStates["fly"]
    if optionStates["fly"] then
        optionFuncs["fly"].conn = RunService.RenderStepped:Connect(function()
            if optionStates["fly"] and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, 40, 0)
            end
        end)
    else
        if optionFuncs["fly"].conn then optionFuncs["fly"].conn:Disconnect() end
    end
end

optionFuncs["velocidade 100"] = function()
    optionStates["velocidade 100"] = not optionStates["velocidade 100"]
    if optionStates["velocidade 100"] then
        LocalPlayer.Character.Humanoid.WalkSpeed = 100
    else
        LocalPlayer.Character.Humanoid.WalkSpeed = 16
    end
end

optionFuncs["esp"] = function()
    optionStates["esp"] = not optionStates["esp"]
    for _,p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character then
            for _,v in pairs(p.Character:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.Color = optionStates["esp"] and Color3.fromRGB(130,130,130) or Color3.fromRGB(255,255,255)
                end
            end
        end
    end
end

optionFuncs["visao no escuro"] = function()
    optionStates["visao no escuro"] = not optionStates["visao no escuro"]
    if optionStates["visao no escuro"] then
        game.Lighting.Brightness = 3
        game.Lighting.ClockTime = 14
        game.Lighting.FogEnd = 100000
    else
        game.Lighting.Brightness = 1
        game.Lighting.ClockTime = 0
        game.Lighting.FogEnd = 1000
    end
end

optionFuncs["teleporte"] = function()
    optionStates["teleporte"] = not optionStates["teleporte"]
    if optionStates["teleporte"] then
        optionFuncs["teleporte"].conn = Mouse.KeyDown:Connect(function(key)
            if key:lower() == "y" then
                if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(Mouse.Hit.p)
                end
            end
        end)
    else
        if optionFuncs["teleporte"].conn then optionFuncs["teleporte"].conn:Disconnect() end
    end
end

optionFuncs["godmode"] = function()
    optionStates["godmode"] = not optionStates["godmode"]
    if optionStates["godmode"] then
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.Health = math.huge
            LocalPlayer.Character.Humanoid.MaxHealth = math.huge
            optionFuncs["godmode"].conn = LocalPlayer.Character.Humanoid:GetPropertyChangedSignal("Health"):Connect(function()
                if optionStates["godmode"] then
                    LocalPlayer.Character.Humanoid.Health = math.huge
                end
            end)
        end
    else
        if optionFuncs["godmode"].conn then optionFuncs["godmode"].conn:Disconnect() end
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.MaxHealth = 100
            LocalPlayer.Character.Humanoid.Health = 100
        end
    end
end

optionFuncs["no slow"] = function()
    optionStates["no slow"] = not optionStates["no slow"]
    if optionStates["no slow"] then
        optionFuncs["no slow"].conn = LocalPlayer.Character.Humanoid:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
            if optionStates["no slow"] then
                LocalPlayer.Character.Humanoid.WalkSpeed = 16
            end
        end)
    else
        if optionFuncs["no slow"].conn then optionFuncs["no slow"].conn:Disconnect() end
    end
end

optionFuncs["creditos"] = setCredits -- créditos não precisa de toggle

-- Altere este trecho na criação dos botões:
btn.MouseButton1Click:Connect(function()
    if v == "creditos" then
        setCredits()
    else
        if optionFuncs[v] then
            optionFuncs[v]()
            -- (Opcional: Mude a cor do botão se estiver ativado)
            btn.BackgroundColor3 = optionStates[v] and Color3.fromRGB(40,140,40) or Color3.fromRGB(20,20,20)
        end
    end
end)
