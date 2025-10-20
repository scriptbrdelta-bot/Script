-- Δ DELTA SCRIPT - SISTEMA DE KEYS MÓVEL (v5) -- 
-- Feito com 💀 por ChatGPT & João -- 
-- v5: somente a key gerada por GET KEY é válida; qualquer outra falha
-- TEMA MODERNO 🔷 - COMPACTO

-- Serviços local 
Players = game:GetService("Players") 
TweenService = game:GetService("TweenService") 
GuiService = game:GetService("GuiService")

-- Constantes 
local WEBSITE = "https://illustrious-figolla-ec7138.netlify.app/"

-- Estado local 
GENERATED_KEY = nil -- só a key gerada aqui será aceita

-- Gera uma key no formato DELTA-XXXXXXX (7 chars alfanum) 
local function generateKey() 
    local chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789" 
    local t = {} 
    for i = 1, 7 do 
        local idx = math.random(1, #chars) 
        t[#t+1] = chars:sub(idx, idx) 
    end 
    return "DELTA-" .. table.concat(t) 
end

-- Cria GUI principal 
local gui = Instance.new("ScreenGui") 
gui.Name = "DeltaKeySystem" 
gui.ResetOnSpawn = false 
gui.Parent = Players.LocalPlayer:WaitForChild("PlayerGui")

-- Frame principal (REDUZIDO)
local main = Instance.new("Frame") 
main.Size = UDim2.new(0, 320, 0, 200)  -- Reduzido de 420x270
main.Position = UDim2.new(0.5, -160, 0.5, -100)  -- Ajustado
main.BackgroundColor3 = Color3.fromRGB(15, 25, 40)  -- Azul escuro moderno
main.BorderSizePixel = 0 
main.Active = true 
main.Draggable = true 
main.Parent = gui

-- Arredondar 
local mainCorner = Instance.new("UICorner", main) 
mainCorner.CornerRadius = UDim.new(0, 12)  -- Mais suave

-- Borda neon azul pulsante 
local glow = Instance.new("UIStroke", main) 
glow.Color = Color3.fromRGB(0, 150, 255)  -- Azul moderno
glow.Thickness = 2 
glow.Transparency = 0

-- Título com gradiente moderno (MENOR)
local title = Instance.new("Frame", main) 
title.Size = UDim2.new(1, 0, 0, 40)  -- Reduzido
title.Position = UDim2.new(0, 0, 0, 0) 
title.BackgroundColor3 = Color3.fromRGB(25, 45, 70)
title.BorderSizePixel = 0

local titleCorner = Instance.new("UICorner", title) 
titleCorner.CornerRadius = UDim.new(0, 10)

local titleLabel = Instance.new("TextLabel", title) 
titleLabel.Size = UDim2.new(1, -12, 1, 0) 
titleLabel.Position = UDim2.new(0, 6, 0, 0) 
titleLabel.BackgroundTransparency = 1 
titleLabel.Text = "Δ DELTA SCRIPT" 
titleLabel.TextColor3 = Color3.new(1,1,1) 
titleLabel.Font = Enum.Font.FredokaOne  -- Fonte mais bonita e moderna
titleLabel.TextSize = 18  -- Tamanho ajustado
titleLabel.TextXAlignment = Enum.TextXAlignment.Left

-- Gradient decorativo moderno
local titleGradient = Instance.new("UIGradient", title) 
titleGradient.Color = ColorSequence.new{ 
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 150, 255)),  -- Azul
    ColorSequenceKeypoint.new(1, Color3.fromRGB(100, 200, 255))   -- Azul claro  
} 
titleGradient.Rotation = 45

-- Caixa de key (MENOR)
local keyBox = Instance.new("TextBox", main) 
keyBox.Size = UDim2.new(0.85, 0, 0, 32)  -- Reduzido
keyBox.Position = UDim2.new(0.075, 0, 0.3, 0) 
keyBox.BackgroundColor3 = Color3.fromRGB(25, 40, 60)
keyBox.TextColor3 = Color3.fromRGB(200, 230, 255)  -- Azul claro
keyBox.PlaceholderText = "Digite ou cole sua key aqui..." 
keyBox.PlaceholderColor3 = Color3.fromRGB(140, 180, 220) 
keyBox.Font = Enum.Font.GothamMedium  -- Fonte melhorada
keyBox.TextSize = 14  -- Menor

local keyCorner = Instance.new("UICorner", keyBox) 
keyCorner.CornerRadius = UDim.new(0, 8)

-- Botões (container) (MENOR)
local btnFrame = Instance.new("Frame", main) 
btnFrame.Size = UDim2.new(0.85, 0, 0, 34)  -- Reduzido
btnFrame.Position = UDim2.new(0.075, 0, 0.55, 0) 
btnFrame.BackgroundTransparency = 1

-- Get Key button (esquerda) 
local getKeyBtn = Instance.new("TextButton", btnFrame) 
getKeyBtn.Size = UDim2.new(0.48, -4, 1, 0)  -- Ajustado
getKeyBtn.Position = UDim2.new(0, 0, 0, 0) 
getKeyBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 255)  -- Azul vibrante
getKeyBtn.Text = "🔑 GET KEY" 
getKeyBtn.Font = Enum.Font.GothamBold 
getKeyBtn.TextSize = 13  -- Menor
getKeyBtn.TextColor3 = Color3.new(1,1,1)

local getCorner = Instance.new("UICorner", getKeyBtn) 
getCorner.CornerRadius = UDim.new(0, 8)

-- Confirm button (direita) 
local confirmBtn = Instance.new("TextButton", btnFrame) 
confirmBtn.Size = UDim2.new(0.48, -4, 1, 0)  -- Ajustado
confirmBtn.Position = UDim2.new(0.52, 8, 0, 0) 
confirmBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 120)  -- Verde moderno
confirmBtn.Text = "✅ CONFIRMAR" 
confirmBtn.Font = Enum.Font.GothamBold 
confirmBtn.TextSize = 13  -- Menor
confirmBtn.TextColor3 = Color3.new(1,1,1)

local confirmCorner = Instance.new("UICorner", confirmBtn) 
confirmCorner.CornerRadius = UDim.new(0, 8)

-- Label de status (MENOR)
local status = Instance.new("TextLabel", main) 
status.Size = UDim2.new(1, 0, 0, 24)  -- Reduzido
status.Position = UDim2.new(0, 0, 0.78, 0) 
status.BackgroundTransparency = 1 
status.Text = "" 
status.Font = Enum.Font.GothamMedium  -- Fonte melhorada
status.TextSize = 12  -- Menor
status.TextColor3 = Color3.fromRGB(180, 220, 255)  -- Azul claro

-- Ícones modernos (MENORES)
local deltaIcon = Instance.new("ImageLabel", main) 
deltaIcon.Size = UDim2.new(0, 22, 0, 22)  -- Reduzido
deltaIcon.Position = UDim2.new(0.04, 0, 0.15, 0) 
deltaIcon.BackgroundTransparency = 1 
deltaIcon.Image = "rbxassetid://7733960981"  -- Substitua por ícone delta se tiver
deltaIcon.ImageColor3 = Color3.fromRGB(0, 150, 255)  -- Azul
deltaIcon.ImageTransparency = 0

local shieldIcon = Instance.new("ImageLabel", main) 
shieldIcon.Size = UDim2.new(0, 20, 0, 20)  -- Reduzido
shieldIcon.Position = UDim2.new(0.89, 0, 0.15, 0) 
shieldIcon.BackgroundTransparency = 1 
shieldIcon.Image = "rbxassetid://7733960981"  -- Substitua por ícone de escudo se tiver
shieldIcon.ImageColor3 = Color3.fromRGB(0, 180, 120)  -- Verde
shieldIcon.Rotation = 0 
shieldIcon.ImageTransparency = 0

-- REMOVIDAS as bolhas decorativas para economizar espaço

-- Função de animação de clique simples 
local function animateClick(btn, normalColor, pressColor) 
    local down = TweenService:Create(btn, TweenInfo.new(0.09), { BackgroundColor3 = pressColor, Size = btn.Size - UDim2.new(0, 4, 0, 2) }) 
    local up = TweenService:Create(btn, TweenInfo.new(0.09), { BackgroundColor3 = normalColor, Size = btn.Size }) 
    down:Play() 
    down.Completed:Wait() 
    up:Play() 
end

-- Função: abrir site e gerar key (GET KEY) 
local function onGetKey() 
    animateClick(getKeyBtn, Color3.fromRGB(0,120,255), Color3.fromRGB(100,180,255)) 
    status.Text = "🔄 Abrindo site..." 
    status.TextColor3 = Color3.fromRGB(180, 220, 255)

    -- gera key e guarda 
    GENERATED_KEY = generateKey() 
    -- abre o site (apenas tentativa) 
    pcall(function() 
        GuiService:OpenBrowserWindow(WEBSITE) 
    end) 
    -- copia link + key para o clipboard caso setclipboard exista 
    local full = WEBSITE .. "?key=" .. GENERATED_KEY 
    if setclipboard then 
        pcall(function() 
            setclipboard(full) 
        end) 
        status.Text = "✅ Link copiado!" 
        status.TextColor3 = Color3.fromRGB(180, 255, 200) 
    else 
        keyBox.Text = GENERATED_KEY 
        status.Text = "📋 Cole o link no navegador" 
        status.TextColor3 = Color3.fromRGB(200, 240, 255) 
    end 
    -- anima ícones leves ao abrir 
    TweenService:Create(deltaIcon, TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {ImageTransparency = 0.2}):Play() 
    TweenService:Create(shieldIcon, TweenInfo.new(0.6, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Rotation = shieldIcon.Rotation + 180}):Play() 
    task.delay(2.2, function() 
        if setclipboard then 
            keyBox.Text = "" 
        end 
        status.Text = "" 
    end) 
end

-- Função: confirmar key (agora valida contra GENERATED_KEY) 
local function onConfirm() 
    animateClick(confirmBtn, Color3.fromRGB(0,180,120), Color3.fromRGB(100,220,160)) 
    local text = tostring(keyBox.Text or "") 
    text = text:gsub("^%s*(.-)%s*$", "%1") -- trim

    -- Se não foi gerada nenhuma key ainda, automaticamente falha 
    if not GENERATED_KEY then 
        status.Text = "❌ Use GET KEY primeiro" 
        status.TextColor3 = Color3.fromRGB(255,120,120) 
        -- shake 
        local orig = keyBox.Position 
        for i = 1, 4 do 
            TweenService:Create(keyBox, TweenInfo.new(0.04, Enum.EasingStyle.Linear), {Position = UDim2.new(orig.X.Scale, orig.X.Offset + (i%2==0 and 4 or -4), orig.Y.Scale, orig.Y.Offset)}):Play() 
            wait(0.04) 
        end 
        TweenService:Create(keyBox, TweenInfo.new(0.06), {Position = orig}):Play() 
        task.delay(1.8, function() status.Text = "" end) 
        return 
    end 
    -- Se campo vazio => erro 
    if text == "" then 
        status.Text = "❌ Insira a key" 
        status.TextColor3 = Color3.fromRGB(255,120,120) 
        -- shake 
        local orig = keyBox.Position 
        for i = 1, 4 do 
            TweenService:Create(keyBox, TweenInfo.new(0.04, Enum.EasingStyle.Linear), {Position = UDim2.new(orig.X.Scale, orig.X.Offset + (i%2==0 and 4 or -4), orig.Y.Scale, orig.Y.Offset)}):Play() 
            wait(0.04) 
        end 
        TweenService:Create(keyBox, TweenInfo.new(0.06), {Position = orig}):Play() 
        task.delay(1.8, function() status.Text = "" end) 
        return 
    end 
    -- Agora valida: somente a GENERATED_KEY é aceita 
    if text == GENERATED_KEY then 
        status.Text = "✅ Key confirmada!" 
        status.TextColor3 = Color3.fromRGB(180, 255, 200) 
        -- animações positivas 
        TweenService:Create(deltaIcon, TweenInfo.new(0.45, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {ImageTransparency = 0}):Play() 
        TweenService:Create(shieldIcon, TweenInfo.new(0.8, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Rotation = shieldIcon.Rotation + 360}):Play() 
        -- opcional: "consome" a key (torna inválida depois de usada) 
        GENERATED_KEY = nil 
    else 
        -- falha — qualquer outra key dá erro 
        status.Text = "❌ Key inválida" 
        status.TextColor3 = Color3.fromRGB(255,120,120) 
        -- shake 
        local orig = keyBox.Position 
        for i = 1, 4 do 
            TweenService:Create(keyBox, TweenInfo.new(0.04, Enum.EasingStyle.Linear), {Position = UDim2.new(orig.X.Scale, orig.X.Offset + (i%2==0 and 4 or -4), orig.Y.Scale, orig.Y.Offset)}):Play() 
            wait(0.04) 
        end 
        TweenService:Create(keyBox, TweenInfo.new(0.06), {Position = orig}):Play() 
    end 
    task.delay(1.8, function() 
        status.Text = "" 
        -- limpa campo após tentativa (para forçar nova geração) 
        keyBox.Text = "" 
    end) 
end

-- Eventos 
getKeyBtn.MouseButton1Click:Connect(onGetKey) 
confirmBtn.MouseButton1Click:Connect(onConfirm)

-- Hover effects 
getKeyBtn.MouseEnter:Connect(function() 
    TweenService:Create(getKeyBtn, TweenInfo.new(0.12), {BackgroundColor3 = Color3.fromRGB(50, 150, 255)}):Play() 
end) 
getKeyBtn.MouseLeave:Connect(function() 
    TweenService:Create(getKeyBtn, TweenInfo.new(0.12), {BackgroundColor3 = Color3.fromRGB(0, 120, 255)}):Play() 
end) 
confirmBtn.MouseEnter:Connect(function() 
    TweenService:Create(confirmBtn, TweenInfo.new(0.12), {BackgroundColor3 = Color3.fromRGB(100, 200, 160)}):Play() 
end) 
confirmBtn.MouseLeave:Connect(function() 
    TweenService:Create(confirmBtn, TweenInfo.new(0.12), {BackgroundColor3 = Color3.fromRGB(0, 180, 120)}):Play() 
end)

-- Animações contínuas decorativas (SIMPLIFICADAS)
spawn(function() 
    while true do 
        -- glow thickness pulse 
        TweenService:Create(glow, TweenInfo.new(1.2), {Thickness = 2.5}):Play() 
        wait(0.8) 
        TweenService:Create(glow, TweenInfo.new(1.2), {Thickness = 2}):Play()

        -- delta icon pulse 
        TweenService:Create(deltaIcon, TweenInfo.new(1.6), {ImageTransparency = 0.4}):Play() 
        wait(0.9) 
        TweenService:Create(deltaIcon, TweenInfo.new(1.6), {ImageTransparency = 0}):Play() 
        -- shield float 
        TweenService:Create(shieldIcon, TweenInfo.new(3.5), {Rotation = shieldIcon.Rotation + 90}):Play() 
        wait(1.2) 
    end 
end)

print("✅ Delta Script Compacto carregado! 🔷")
