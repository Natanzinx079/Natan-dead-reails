-- NATAN DEAD REAILS 1.0 - SUPER SCRIPT

-- GUI PRINCIPAL
local plr = game.Players.LocalPlayer
local ScreenGui = Instance.new("ScreenGui", plr.PlayerGui)
ScreenGui.Name = "NatanDeadRailsUI"

-- TÍTULO
local Title = Instance.new("TextLabel", ScreenGui)
Title.Size = UDim2.new(0, 320, 0, 50)
Title.Position = UDim2.new(0.5, -160, 0, 10)
Title.Text = "NATAN DEAD REAILS 1.0"
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Title.TextScaled = true
Title.Font = Enum.Font.GothamBlack

-- Função para criar botão
local function createButton(txt, posY, callback)
    local btn = Instance.new("TextButton", ScreenGui)
    btn.Size = UDim2.new(0, 240, 0, 30)
    btn.Position = UDim2.new(0, 50, 0, posY)
    btn.Text = txt
    btn.BackgroundColor3 = Color3.fromHSV(math.random(), 1, 1)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.TextScaled = true
    btn.Font = Enum.Font.GothamBold
    btn.MouseButton1Click:Connect(callback)
end

-- Teleporte
local locations = {
    ["Trem"] = Vector3.new(0,10,0),
    ["Base 1"] = Vector3.new(100,10,200),
    ["Base 2"] = Vector3.new(300,10,500),
    ["Base 3"] = Vector3.new(600,10,800),
    ["Final"] = Vector3.new(1000,10,1000)
}

local offset = 60
local i = 0
for name, pos in pairs(locations) do
    createButton("Ir para " .. name, offset + i*40, function()
        local hrp = plr.Character and plr.Character:FindFirstChild("HumanoidRootPart")
        if hrp then hrp.CFrame = CFrame.new(pos) end
    end)
    i += 1
end

-- Noclip
local noclip = false
createButton("Ativar/Desativar Noclip", offset + i*40, function()
    noclip = not noclip
end)

game:GetService("RunService").Stepped:Connect(function()
    if noclip and plr.Character then
        for _, v in pairs(plr.Character:GetDescendants()) do
            if v:IsA("BasePart") then
                v.CanCollide = false
            end
        end
    end
end)

-- ESP
createButton("Ativar ESP", offset + (i+1)*40, function()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= plr and player.Character and not player.Character:FindFirstChild("ESP") then
            local esp = Instance.new("BillboardGui", player.Character)
            esp.Name = "ESP"
            esp.Size = UDim2.new(0, 100, 0, 40)
            esp.AlwaysOnTop = true
            esp.Adornee = player.Character:FindFirstChild("Head")
            local text = Instance.new("TextLabel", esp)
            text.Size = UDim2.new(1, 0, 1, 0)
            text.BackgroundTransparency = 1
            text.Text = player.Name
            text.TextColor3 = Color3.fromRGB(255, 0, 0)
            text.TextScaled = true
        end
    end
end)

-- Auto Kill (teleporta para inimigos próximos)
createButton("Auto Kill", offset + (i+2)*40, function()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= plr and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local target = player.Character.HumanoidRootPart
            local myChar = plr.Character and plr.Character:FindFirstChild("HumanoidRootPart")
            if myChar then
                myChar.CFrame = target.CFrame + Vector3.new(0, 0, 2)
                wait(0.2)
            end
        end
    end
end)

-- Aumentar velocidade
createButton("Velocidade 100", offset + (i+3)*40, function()
    local humanoid = plr.Character and plr.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then humanoid.WalkSpeed = 100 end
end)

-- Invisibilidade
createButton("Ficar Invisível", offset + (i+4)*40, function()
    if plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
        plr.Character.HumanoidRootPart.Transparency = 1
        for _, part in pairs(plr.Character:GetDescendants()) do
            if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                part.Transparency = 1
            end
        end
    end
end)
