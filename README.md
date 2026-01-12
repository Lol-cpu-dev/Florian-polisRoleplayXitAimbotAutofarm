--==================================================
-- FPR ZORION HUB - COM FUNÇÃO TESTE
--==================================================

-- RAYFIELD UI
local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()
local Window = Rayfield:CreateWindow({
    Name = "FPR ZORION HUB",
    LoadingTitle = "Carregando Sistema Completo...",
    LoadingSubtitle = "Com Função Teste",
    ConfigurationSaving = {Enabled = false}
})

-- SERVICES
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local Workspace = game:GetService("Workspace")
local lp = Players.LocalPlayer

-- VARIÁVEIS GLOBAIS
local FARM_LIXEIRO = false
local FARM_MERCADO = false
local SPEED_ATIVO = false
local VELOCIDADE_NORMAL = 16
local TELA_ESTICADA_ATIVO = false
local FORCED_FOV = 120
local AIMBOT_ESP_ATIVO = false
local INSTANT_PROMPT_ATIVO = false
local INFINITE_YIELD_ATIVO = false

-- ================================
-- FUNÇÃO TESTE - CRIAR 5 MODELOS
-- ================================
local function criarTesteModelos()
    local modeloOriginal = workspace.Floripa_workspace.Empregos.Caixa.Caixa.Sacola
    
    if not modeloOriginal then
        Rayfield:Notify({
            Title = "❌ MODELO NÃO ENCONTRADO",
            Content = "Não foi possível encontrar a Sacola",
            Duration = 3
        })
        return
    end
    
    -- Posição inicial
    local posicaoBase = modeloOriginal.Position
    
    -- Criar 4 cópias extras
    for i = 1, 4 do
        task.spawn(function()
            -- Clonar o modelo
            local clone = modeloOriginal:Clone()
            
            -- Posicionar em linha
            local offsetX = i * 4 -- 4 studs de distância entre cada
            clone.Position = posicaoBase + Vector3.new(offsetX, 0, 0)
            
            -- Renomear para identificação
            clone.Name = "Sacola_Clone_" .. i
            
            -- Adicionar efeito visual
            if clone:FindFirstChildOfClass("Part") then
                for _, part in ipairs(clone:GetDescendants()) do
                    if part:IsA("BasePart") then
                        -- Cor diferente para cada clone
                        if i == 1 then part.BrickColor = BrickColor.new("Bright red")
                        elseif i == 2 then part.BrickColor = BrickColor.new("Bright blue")
                        elseif i == 3 then part.BrickColor = BrickColor.new("Bright green")
                        elseif i == 4 then part.BrickColor = BrickColor.new("Bright yellow") end
                        
                        part.Material = EnumMaterial.Neon
                        part.Transparency = 0.3
                    end
                end
            end
            
            -- Adicionar ProximityPrompt se não tiver
            if not clone:FindFirstChild("ProximityPrompt") then
                local prompt = Instance.new("ProximityPrompt")
                prompt.Name = "ProximityPrompt"
                prompt.ActionText = "Usar Sacola Teste " .. i
                prompt.ObjectText = "Sacola Clone"
                prompt.HoldDuration = 0.5
                prompt.MaxActivationDistance = 10
                prompt.Parent = clone
            end
            
            -- Adicionar brilho
            local highlight = Instance.new("Highlight")
            highlight.Name = "HighlightEffect"
            highlight.FillColor = Color3.new(1, 0.5, 0)
            highlight.FillTransparency = 0.7
            highlight.OutlineColor = Color3.new(1, 1, 0)
            highlight.OutlineTransparency = 0.3
            highlight.Parent = clone
            
            -- Parentar no mesmo local do original
            clone.Parent = modeloOriginal.Parent
            
            print("✅ Clone " .. i .. " criado: " .. clone.Name)
        end)
        
        -- Pequeno delay entre clones
        task.wait(0.2)
    end
    
    -- Efeito visual na sacola original
    if modeloOriginal:FindFirstChildOfClass("Part") then
        for _, part in ipairs(modeloOriginal:GetDescendants()) do
            if part:IsA("BasePart") then
                part.BrickColor = BrickColor.new("Bright orange")
                part.Material = EnumMaterial.Neon
                
                -- Piscar efeito
                for _ = 1, 3 do
                    part.Transparency = 0.2
                    task.wait(0.2)
                    part.Transparency = 0
                    task.wait(0.2)
                end
            end
        end
    end
    
    Rayfield:Notify({
        Title = "✅ TESTE CRIADO COM SUCESSO",
        Content = "4 clones criados ao lado da sacola original\nCores: Vermelho, Azul, Verde, Amarelo",
        Duration = 5
    })
    
    print("========================================")
    print("FUNÇÃO TESTE EXECUTADA")
    print("========================================")
    print("✅ Sacola original: " .. modeloOriginal.Name)
    print("✅ 4 clones criados em linha")
    print("✅ Cores diferentes para cada")
    print("✅ ProximityPrompt adicionado")
    print("✅ Highlight visual")
    print("========================================")
end

-- ================================
-- FUNÇÃO LIMPAR TESTE
-- ================================
local function limparTesteModelos()
    local parent = workspace.Floripa_workspace.Empregos.Caixa.Caixa
    
    -- Encontrar e remover clones
    local clonesRemovidos = 0
    for _, child in ipairs(parent:GetChildren()) do
        if child.Name:find("Sacola_Clone_") then
            child:Destroy()
            clonesRemovidos = clonesRemovidos + 1
        end
    end
    
    -- Resetar sacola original
    local sacolaOriginal = parent:FindFirstChild("Sacola")
    if sacolaOriginal then
        for _, part in ipairs(sacolaOriginal:GetDescendants()) do
            if part:IsA("BasePart") then
                part.BrickColor = BrickColor.new("Bright orange")
                part.Transparency = 0
                part.Material = EnumMaterial.Plastic
            end
        end
    end
    
    Rayfield:Notify({
        Title = "🧹 TESTE LIMPO",
        Content = clonesRemovidos .. " clones removidos\nSacola original resetada",
        Duration = 3
    })
    
    print("🧹 " .. clonesRemovidos .. " clones removidos")
end

-- ================================
-- FUNÇÃO INFINITE YIELD
-- ================================
local function ativarInfiniteYield()
    if INFINITE_YIELD_ATIVO then
        Rayfield:Notify({
            Title = "⏸️ INFINITE YIELD DESATIVADO",
            Content = "Feche o console para desativar",
            Duration = 3
        })
        return
    end
    
    local sucesso, erro = pcall(function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source', true))()
    end)
    
    if sucesso then
        INFINITE_YIELD_ATIVO = true
        Rayfield:Notify({
            Title = "✅ INFINITE YIELD ATIVADO",
            Content = "Pressione ';' para abrir o console",
            Duration = 5
        })
    end
end

-- ================================
-- FUNÇÃO INSTANT PROMPT
-- ================================
local function ativarInstantPrompt()
    if not INSTANT_PROMPT_ATIVO then return end
    
    local function updateProximityPrompts()
        for _, v in ipairs(Workspace:GetDescendants()) do
            if v.ClassName == "ProximityPrompt" then
                v.HoldDuration = 0.0001
            end
        end
    end
    
    updateProximityPrompts()
    
    Workspace.DescendantAdded:Connect(function(descendant)
        if descendant.ClassName == "ProximityPrompt" then
            descendant.HoldDuration = 0.0001
        end
    end)
end

-- ================================
-- TELA ESTICADA (FOV FIXO)
-- ================================
local function aplicarFOVEsticado()
    if not TELA_ESTICADA_ATIVO then return end
    
    local camera = workspace.CurrentCamera
    if camera and camera.FieldOfView ~= FORCED_FOV then
        camera.FieldOfView = FORCED_FOV
    end
end

local function iniciarTelaEsticada()
    if TELA_ESTICADA_ATIVO then
        aplicarFOVEsticado()
        
        lp.CharacterAdded:Connect(function()
            task.wait(0.5)
            aplicarFOVEsticado()
        end)
        
        local bloqueando = false
        workspace.CurrentCamera:GetPropertyChangedSignal("FieldOfView"):Connect(function()
            if bloqueando then return end
            if workspace.CurrentCamera.FieldOfView ~= FORCED_FOV then
                bloqueando = true
                workspace.CurrentCamera.FieldOfView = FORCED_FOV
                task.defer(function()
                    bloqueando = false
                end)
            end
        end)
        
        print("✅ TELA ESTICADA ATIVADA: FOV " .. FORCED_FOV)
    end
end

task.spawn(function()
    while true do
        if TELA_ESTICADA_ATIVO then
            aplicarFOVEsticado()
        end
        task.wait(0.1)
    end
end)

-- ================================
-- TAB 1: FARM
-- ================================
local FarmTab = Window:CreateTab("🚚 Farm", 4483362458)

task.spawn(function()
    while true do
        if FARM_LIXEIRO and lp.Character then
            local hum = lp.Character:FindFirstChild("Humanoid")
            if hum then
                local pegar = workspace.Floripa_workspace.Empregos.Lixeiro.Pega:GetChildren()[3]
                if pegar then
                    local prompt = pegar:FindFirstChild("ProximityPrompt")
                    if prompt and prompt.Enabled then
                        hum:MoveTo(prompt.Parent.Position)
                        task.wait(0.3)
                        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.F, false, game)
                        task.wait(prompt.HoldDuration or 0.3)
                        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.F, false, game)
                    end
                end
                
                local entregar = workspace.Floripa_workspace.Empregos.Lixeiro.Entrega:GetChildren()[2]
                if entregar then
                    local prompt = entregar:FindFirstChild("ProximityPrompt")
                    if prompt and prompt.Enabled then
                        hum:MoveTo(prompt.Parent.Position)
                        task.wait(0.3)
                        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.F, false, game)
                        task.wait(prompt.HoldDuration or 0.3)
                        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.F, false, game)
                    end
                end
            end
        end
        task.wait(0.1)
    end
end)

task.spawn(function()
    while true do
        if FARM_MERCADO and lp.Character then
            local hum = lp.Character:FindFirstChild("Humanoid")
            if hum then
                local caixa = workspace.Floripa_workspace.Empregos.Caixa
                if caixa then
                    local caixa2 = caixa:GetChildren()[2]
                    if caixa2 then
                        local sacola = caixa2:FindFirstChild("Sacola")
                        if sacola then
                            local prompt = sacola:FindFirstChild("ProximityPrompt")
                            if prompt and prompt.Enabled then
                                hum:MoveTo(prompt.Parent.Position)
                                task.wait(0.3)
                                VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.F, false, game)
                                task.wait(prompt.HoldDuration or 0.3)
                                VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.F, false, game)
                            end
                        end
                    end
                end
            end
        end
        task.wait(0.1)
    end
end)

FarmTab:CreateToggle({
    Name = "🚚 Lixeiro",
    CurrentValue = false,
    Callback = function(v)
        FARM_LIXEIRO = v
        if v then FARM_MERCADO = false end
    end
})

FarmTab:CreateToggle({
    Name = "🛒 Mercado",
    CurrentValue = false,
    Callback = function(v)
        FARM_MERCADO = v
        if v then FARM_LIXEIRO = false end
    end
})

FarmTab:CreateToggle({
    Name = "⚡ Instant Prompt (F Instant)",
    CurrentValue = false,
    Callback = function(v)
        INSTANT_PROMPT_ATIVO = v
        if v then ativarInstantPrompt() end
    end
})

-- ================================
-- TAB 2: AIMBOT+ESP
-- ================================
local AimbotTab = Window:CreateTab("🎯 Aimbot+ESP", 9753762469)

local function ativarAimbotESP()
    if AIMBOT_ESP_ATIVO then
        AIMBOT_ESP_ATIVO = false
        return
    end
    
    local sucesso = pcall(function()
        local scriptCode = game:HttpGet("https://raw.githubusercontent.com/randomuser832/Scripts25/refs/heads/main/UniversalAimbotLoadString")
        
        local configurado = scriptCode:gsub('_G%.Sensitivity = 1', '_G.Sensitivity = 0.3')
        configurado = configurado:gsub('_G%.FOVRadius = 60', '_G.FOVRadius = 80')
        
        loadstring(configurado)()
    end)
    
    if sucesso then
        AIMBOT_ESP_ATIVO = true
        Rayfield:Notify({
            Title = "✅ AIMBOT+ESP ATIVADO",
            Content = "Use RightShift para ligar/desligar",
            Duration = 5
        })
    end
end

AimbotTab:CreateButton({
    Name = "🎯 ATIVAR AIMBOT+ESP UNIVERSAL",
    Callback = ativarAimbotESP
})

-- ================================
-- TAB 3: UTILIDADES COM TESTE
-- ================================
local UtilTab = Window:CreateTab("🛠️ Utilidades", 6034509993)

-- SEÇÃO TESTE
UtilTab:CreateSection("🧪 FUNÇÃO TESTE")

UtilTab:CreateButton({
    Name = "🧪 CRIAR TESTE (5 Sacolas)",
    Callback = criarTesteModelos
})

UtilTab:CreateButton({
    Name = "🧹 LIMPAR TESTE",
    Callback = limparTesteModelos
})

UtilTab:CreateLabel("")
UtilTab:CreateLabel("🧪 TESTE - O QUE FAZ:")
UtilTab:CreateLabel("• Cria 4 clones da sacola original")
UtilTab:CreateLabel("• Cada clone tem cor diferente")
UtilTab:CreateLabel("• Posiciona em linha ao lado")
UtilTab:CreateLabel("• Adiciona ProximityPrompt")
UtilTab:CreateLabel("• Highlight visual")

-- SEÇÃO INFINITE YIELD
UtilTab:CreateSection("🌀 INFINITE YIELD")

UtilTab:CreateButton({
    Name = "🌀 ATIVAR INFINITE YIELD",
    Callback = ativarInfiniteYield
})

-- SEÇÃO OUTRAS UTILIDADES
UtilTab:CreateSection("📁 Outras Utilidades")

UtilTab:CreateButton({
    Name = "📋 Copiar Posição Atual",
    Callback = function()
        if lp.Character and lp.Character:FindFirstChild("HumanoidRootPart") then
            local pos = lp.Character.HumanoidRootPart.Position
            local text = string.format("Vector3.new(%.1f, %.1f, %.1f)", pos.X, pos.Y, pos.Z)
            
            pcall(function()
                if syn and syn.write_clipboard then
                    syn.write_clipboard(text)
                elseif setclipboard then
                    setclipboard(text)
                end
            end)
            
            Rayfield:Notify({
                Title = "📋 Posição Copiada",
                Content = text,
                Duration = 5
            })
        end
    end
})

-- ================================
-- TAB 4: VISUAIS
-- ================================
local VisuaisTab = Window:CreateTab("🎨 Visuais", 6034509993)

VisuaisTab:CreateToggle({
    Name = "📺 Tela Esticada (FOV Fixo)",
    CurrentValue = false,
    Callback = function(v)
        TELA_ESTICADA_ATIVO = v
        if v then iniciarTelaEsticada() end
    end
})

VisuaisTab:CreateSlider({
    Name = "📐 FOV da Tela Esticada",
    Range = {90, 140},
    Increment = 5,
    Suffix = "°",
    CurrentValue = 120,
    Callback = function(v)
        FORCED_FOV = v
        if TELA_ESTICADA_ATIVO then aplicarFOVEsticado() end
    end
})

-- SPEED BOTÃO
local speedGui = Instance.new("ScreenGui", lp.PlayerGui)
speedGui.Name = "SpeedButtonGUI"

local speedBtn = Instance.new("TextButton")
speedBtn.Name = "SpeedBtn"
speedBtn.Size = UDim2.new(0, 85, 0, 40)
speedBtn.Position = UDim2.new(0.85, 0, 0.9, 0)
speedBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
speedBtn.Text = "⚡ SPEED 300"
speedBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
speedBtn.TextSize = 14
speedBtn.Font = Enum.Font.GothamBold
speedBtn.Parent = speedGui

local btnCorner = Instance.new("UICorner", speedBtn)
btnCorner.CornerRadius = UDim.new(0, 8)

local btnStroke = Instance.new("UIStroke", speedBtn)
btnStroke.Color = Color3.fromRGB(60, 60, 80)
btnStroke.Thickness = 2

local function toggleSpeed()
    local char = lp.Character
    if not char then return end
    
    local hum = char:FindFirstChild("Humanoid")
    if not hum then return end
    
    if not SPEED_ATIVO then
        VELOCIDADE_NORMAL = hum.WalkSpeed
        hum.WalkSpeed = 300
        SPEED_ATIVO = true
        
        speedBtn.Text = "⚡ SPEED ON"
        speedBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 0)
        btnStroke.Color = Color3.fromRGB(0, 255, 0)
    else
        hum.WalkSpeed = VELOCIDADE_NORMAL
        SPEED_ATIVO = false
        
        speedBtn.Text = "⚡ SPEED 300"
        speedBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
        btnStroke.Color = Color3.fromRGB(60, 60, 80)
    end
end

speedBtn.MouseButton1Click:Connect(toggleSpeed)

VisuaisTab:CreateToggle({
    Name = "⚡ Speed 300",
    CurrentValue = false,
    Callback = function(v)
        SPEED_ATIVO = v
        toggleSpeed()
    end
})

-- ================================
-- TAB 5: INFO
-- ================================
local InfoTab = Window:CreateTab("📋 Info", 4483362458)

InfoTab:CreateLabel("🔥 FPR ZORION HUB COMPLETO")
InfoTab:CreateLabel("")
InfoTab:CreateLabel("✅ RECURSOS INCLUÍDOS:")
InfoTab:CreateLabel("🚚 Farm: Lixeiro e Mercado")
InfoTab:CreateLabel("⚡ Instant Prompt: F instantâneo")
InfoTab:CreateLabel("🎯 Aimbot+ESP: Sistema Universal")
InfoTab:CreateLabel("🧪 Teste: Cria 5 sacolas coloridas")
InfoTab:CreateLabel("🌀 Infinite Yield: Console de comandos")
InfoTab:CreateLabel("📺 Tela Esticada: FOV fixo")
InfoTab:CreateLabel("⚡ Speed: Botão arrastável 300")
InfoTab:CreateLabel("")
InfoTab:CreateLabel("🧪 FUNÇÃO TESTE:")
InfoTab:CreateLabel("• Cria 4 clones da sacola do mercado")
InfoTab:CreateLabel("• Cada clone tem cor diferente")
InfoTab:CreateLabel("• Serve para testar autofarm")
InfoTab:CreateLabel("• Use na aba Utilidades")
InfoTab:CreateLabel("")
InfoTab:CreateLabel("⚡ Por: Zorion")

-- ================================
-- INICIALIZAÇÃO
-- ================================
Rayfield:Notify({
    Title = "🔥 FPR ZORION HUB COMPLETO",
    Content = "Sistema carregado!\nFunção Teste adicionada",
    Duration = 5
})

print("========================================")
print("FPR ZORION HUB - COM FUNÇÃO TESTE")
print("========================================")
print("🧪 NOVA FUNÇÃO TESTE:")
print("• Cria 4 clones da sacola do mercado")
print("• Cada clone: Vermelho, Azul, Verde, Amarelo")
print("• Posicionados em linha ao lado da original")
print("• Com ProximityPrompt para testar farm")
print("• Highlight visual para fácil identificação")
print("========================================")
