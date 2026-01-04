local _0x5F2 = loadstring(game:HttpGet(utf8.decode("\104\116\116\112\115\58\47\47\115\105\114\105\117\115\46\109\101\110\117\47\114\97\121\102\105\101\108\100")))()

local _0x1A = _0x5F2:CreateWindow({
   Name = "jotinha12hr2 hub's Admin (Client-Side)",
   LoadingTitle = "carregando, by jotinha12hr2",
   LoadingSubtitle = "by jotinha12hr2",
   ConfigurationSaving = { Enabled = false }
})

local _0xTab1 = _0x1A:CreateTab("Admin Players", 4483362458)
local _0xTarget = ""

local _0xList = {
    "jotinha12hr2 hub's Admin",
    "coquette hub admin",
    "lyra hub Admin",
    "drip client admin",
    "Phantom cliente Admin"
}

-- Verificação: Só funciona se o alvo tiver um dos itens da lista
local function _0xCheck(plr)
    for _, h in ipairs(_0xList) do
        if plr:FindFirstChild(h) or (plr:FindFirstChild("PlayerGui") and plr.PlayerGui:FindFirstChild(h)) or plr.Character:FindFirstChild(h) then
            return true
        end
    end
    return false
end

local function _0xGetP()
    for _, v in pairs(game.Players:GetPlayers()) do
        if v.Name:lower():sub(1, #_0xTarget) == _0xTarget:lower() or v.DisplayName:lower():sub(1, #_0xTarget) == _0xTarget:lower() then
            return v
        end
    end
end

_0xTab1:CreateInput({
   Name = "Nome do Player", PlaceholderText = "Escreva o nome...",
   RemoveTextAfterFocusLost = false,
   Callback = function(t) _0xTarget = t end,
})

-- Lógica de Execução (Tenta burlar sem SS via NetworkOwnership se possível)
local function _0xAct(n, f)
    _0xTab1:CreateButton({
       Name = n,
       Callback = function()
          local p = _0xGetP()
          if p and _0xCheck(p) then
              if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then 
                  f(p) 
              end
          else
              _0x5F2:Notify({Title = "Aviso", Content = "Alvo não usa os Hubs ou não foi encontrado.", Duration = 3})
          end
       end,
    })
end

-- COMANDOS AJUSTADOS PARA FUNCIONAR VIA CLIENT (TENTATIVA)
_0xAct("Kick Player (Client)", function(p) 
    -- Tenta crashar o cliente do alvo enviando valores inválidos se ele estiver usando o Hub
    p:Destroy() 
end)

_0xAct("Kill Player (Exploit)", function(p)
    -- Tenta deletar o pescoço ou quebrar juntas localmente (Só funciona se o NetworkOwnership permitir)
    if p.Character:FindFirstChild("Head") then
        p.Character.Head:Destroy()
    end
end)

_0xAct("Bring Player (Simulação)", function(p)
    -- Tenta puxar o HumanoidRootPart do alvo para você
    local lp = game.Players.LocalPlayer
    if lp.Character and lp.Character:FindFirstChild("HumanoidRootPart") then
        p.Character.HumanoidRootPart.CFrame = lp.Character.HumanoidRootPart.CFrame
    end
end)

_0xAct("Controlar (Not FE)", function(p)
    game.Players.LocalPlayer.Character = p.Character
    workspace.CurrentCamera.CameraSubject = p.Character.Humanoid
end)

_0xAct("Freeze Player", function(p)
    p.Character.HumanoidRootPart.Anchored = true
end)

_0xAct("Unfreeze Player", function(p)
    p.Character.HumanoidRootPart.Anchored = false
end)

_0xAct("Void Player", function(p)
    p.Character.HumanoidRootPart.CFrame = CFrame.new(0, -500, 0)
end)

