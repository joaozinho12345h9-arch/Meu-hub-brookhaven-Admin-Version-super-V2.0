-- Script Menu de Admin com Lógica
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Variáveis de Controle
local targetPlayerName = ""
local controlConnection = nil

-- --- CRIAÇÃO DA INTERFACE (Baseado no seu modelo) ---
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AdminMenuSystem"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 300, 0, 450)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -225)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.Visible = false
mainFrame.Active = true
mainFrame.Draggable = true -- Permite arrastar o menu
mainFrame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.Parent = mainFrame

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
title.Text = "PAINEL ADMIN"
title.TextColor3 = Color3.white
title.Font = Enum.Font.SourceSansBold
title.TextSize = 20
title.Parent = mainFrame

local playerTextBox = Instance.new("TextBox")
playerTextBox.Size = UDim2.new(0.9, 0, 0, 35)
playerTextBox.Position = UDim2.new(0.05, 0, 0, 50)
playerTextBox.PlaceholderText = "Nome do Player Alvo..."
playerTextBox.Text = ""
playerTextBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
playerTextBox.TextColor3 = Color3.white
playerTextBox.Parent = mainFrame

local scrollFrame = Instance.new("ScrollingFrame")
scrollFrame.Size = UDim2.new(0.9, 0, 0, 330)
scrollFrame.Position = UDim2.new(0.05, 0, 0, 100)
scrollFrame.BackgroundTransparency = 1
scrollFrame.CanvasSize = UDim2.new(0, 0, 1.5, 0)
scrollFrame.ScrollBarThickness = 4
scrollFrame.Parent = mainFrame

local layout = Instance.new("UIListLayout")
layout.Padding = UDim.new(0, 5)
layout.Parent = scrollFrame

-- --- FUNÇÃO PARA CRIAR BOTÕES ---
local function createBtn(name, color, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -10, 0, 35)
    btn.BackgroundColor3 = color
    btn.Text = name
    btn.TextColor3 = Color3.white
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 16
    btn.Parent = scrollFrame
    
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, 6)
    c.Parent = btn
    
    btn.MouseButton1Click:Connect(function()
        local target = nil
        -- Busca o player pelo nome aproximado
        for _, p in pairs(Players:GetPlayers()) do
            if string.find(p.Name:lower(), playerTextBox.Text:lower()) then
                target = p
                break
            end
        end
        callback(target)
    end)
end

-- --- LÓGICA DAS FUNÇÕES ---

createBtn("Kick", Color3.fromRGB(200, 50, 50), function(t)
    if t then t:Kick("Você foi removido pelo admin.") end
end)

createBtn("Kill", Color3.fromRGB(150, 0, 0), function(t)
    if t and t.Character then t.Character:BreakJoints() end
end)

createBtn("Kill Plus (Explode)", Color3.fromRGB(255, 0, 0), function(t)
    if t and t.Character then
        local ex = Instance.new("Explosion")
        ex.Position = t.Character.HumanoidRootPart.Position
        ex.Parent = game.Workspace
    end
end)

createBtn("Jumpscare 1 (Screamer)", Color3.fromRGB(100, 0, 200), function(t)
    print("Enviando Jumpscare 1 para " .. (t and t.Name or "Ninguém"))
    -- Nota: Jumpscares visuais exigem RemoteEvents ou scripts locais no alvo
end)

createBtn("Jumpscare 2", Color3.fromRGB(120, 0, 220), function(t)
     -- Lógica de áudio alto aqui
end)

createBtn("Jumpscare 3", Color3.fromRGB(140, 0, 240), function(t)
    -- Lógica de imagem na tela aqui
end)

createBtn("Jail", Color3.fromRGB(100, 100, 100), function(t)
    if t and t.Character then
        t.Character.HumanoidRootPart.Anchored = true
        local part = Instance.new("Part", game.Workspace)
        part.Name = "JailCell"
        part.Size = Vector3.new(6, 10, 6)
        part.Position = t.Character.HumanoidRootPart.Position
        part.Transparency = 0.5
        part.CanCollide = true
    end
end)

createBtn("UnJail", Color3.fromRGB(0, 150, 0), function(t)
    if t and t.Character then
        t.Character.HumanoidRootPart.Anchored = false
    end
end)

createBtn("Controlar Personagem", Color3.fromRGB(0, 100, 255), function(t)
    if t and t.Character then
        player.Character = t.Character
        workspace.CurrentCamera.CameraSubject = t.Character.Humanoid
    end
end)

-- Botão Abrir/Fechar
local openBtn = Instance.new("TextButton")
openBtn.Size = UDim2.new(0, 50, 0, 50)
openBtn.Position = UDim2.new(0, 10, 0.5, -25)
openBtn.Text = "ADMIN"
openBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
openBtn.TextColor3 = Color3.white
openBtn.Parent = screenGui

local c2 = Instance.new("UICorner")
c2.CornerRadius = UDim.new(1, 0)
c2.Parent = openBtn

openBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = not mainFrame.Visible
end)
