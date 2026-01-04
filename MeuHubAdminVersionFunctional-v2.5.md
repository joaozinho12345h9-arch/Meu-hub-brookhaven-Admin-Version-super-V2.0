local _0xR = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local _0xW = _0xR:CreateWindow({Name="jotinha12hr2 hub's Admin",LoadingTitle="Modo Aberto Ativo...",LoadingSubtitle="sem bloqueios",ConfigurationSaving={Enabled=false}})
local _0xT = _0xW:CreateTab("Admin Players", 4483362458)

local _0xP = ""
local _0xS = "[JOTA_OPEN]:"
local _0xMe = game.Players.LocalPlayer

-- Função de execução direta (sem verificações de ID)
local function _0xExec(_0xCmd, _0xTargetName)
    local _0xTargetPlr = game.Players:FindFirstChild(_0xTargetName)
    if not _0xTargetPlr then return end

    -- Execução no Cliente do Alvo (Se o alvo for quem está lendo a mensagem)
    if _0xMe.Name == _0xTargetName then
        if _0xCmd == "KICK" then
            _0xMe:Kick("\n[jotinha12hr2 hub]\nExpulso por um Admin.")
            return
        elseif _0xCmd == "BCK" then
            if _0xMe.Character and _0xMe.Character:FindFirstChild("HumanoidRootPart") then
                _0xMe.Character.HumanoidRootPart.CFrame = CFrame.new(9999, -999, 9999)
            end
        end
    end

    -- Execução Visual/Física Sincronizada
    local _0xChar = _0xTargetPlr.Character
    if not _0xChar then return end

    if _0xCmd == "KILL" then
        _0xChar:BreakJoints()
    elseif _0xCmd == "KILL+" then
        if _0xChar:FindFirstChild("Humanoid") then _0xChar.Humanoid.Health = 0 end
        for _,v in pairs(_0xChar:GetChildren()) do if v:IsA("BasePart") then v:Destroy() end end
    elseif _0xCmd == "JAIL" then
        if workspace:FindFirstChild("Jail_".._0xTargetName) then workspace["Jail_".._0xTargetName]:Destroy() end
        local cf = _0xChar.HumanoidRootPart.CFrame
        local m = Instance.new("Model", workspace); m.Name = "Jail_".._0xTargetName
        local p={Vector3.new(0,-5,0),Vector3.new(0,5,0),Vector3.new(5,0,0),Vector3.new(-5,0,0),Vector3.new(0,0,5),Vector3.new(0,0,-5)}
        for _,v in pairs(p) do
            local pt=Instance.new("Part",m); pt.Size=Vector3.new(10,1,10); pt.CFrame=cf*CFrame.new(v); pt.Anchored=true; pt.Transparency=0.5; pt.Color=Color3.fromRGB(255,105,180)
        end
    elseif _0xCmd == "UNJAIL" then
        if workspace:FindFirstChild("Jail_".._0xTargetName) then workspace["Jail_".._0xTargetName]:Destroy() end
    end
end

-- Sistema de escuta via Chat
game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("OnMessageDoneFiltering").OnClientEvent:Connect(function(_0xData)
    local _0xMsg = _0xData.Message
    if _0xMsg:sub(1, #_0xS) == _0xS then
        local _0xContent = _0xMsg:sub(#_0xS + 1)
        local _0xSplit = _0xContent:split("|")
        _0xExec(_0xSplit[1], _0xSplit[2])
    end
end)

local function _0xSend(_0xType, _0xTarget)
    game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(_0xS.._0xType.."|".._0xTarget, "All")
end

_0xT:CreateInput({Name="Nome do Player", PlaceholderText="Alvo...", Callback=function(t) _0xP = t end})

local function _0xBtn(_0xLabel, _0xType)
    _0xT:CreateButton({Name=_0xLabel, Callback=function()
        local _0xTarg = nil
        for _,v in pairs(game.Players:GetPlayers()) do
            if v.Name:lower():sub(1,#_0xP) == _0xP:lower() then _0xTarg = v break end
        end
        if _0xTarg then
            _0xSend(_0xType, _0xTarg.Name)
            _0xExec(_0xType, _0xTarg.Name)
        end
    end})
end

_0xBtn("Kick (Livre)", "KICK")
_0xBtn("Kill Sync", "KILL")
_0xBtn("Kill+ Sync", "KILL+")
_0xBtn("Jail Sync", "JAIL")
_0xBtn("Unjail Sync", "UNJAIL")
_0xBtn("Backrooms Sync", "BCK")

_0xR:Notify({Title="Atenção", Content="Modo sem bloqueio ativo. Todos os usuários podem se afetar!", Duration=5})

