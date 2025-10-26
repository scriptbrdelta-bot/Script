-- Δ DELTA SCRIPT - SISTEMA DE KEYS MÓVEL (v5) HALLOWEEN EDITION --
-- Feito com 💀 por ChatGPT & João --
-- v5: somente a key gerada por GET KEY é válida; qualquer outra falha --
-- TEMA HALLOWEEN 🎃 - COMPACTO

-- Serviços local
Players = game:GetService("Players")
TweenService = game:GetService("TweenService")
GuiService = game:GetService("GuiService")

-- Constantes
local WEBSITE = "https://cozy-salamander-1ce627.netlify.app/"

-- Estado
local GENERATED_KEY = nil -- só a key gerada aqui será aceita

-- Gera uma key no formato SPOOKY-XXXXXXX (7 chars alfanum)
local function generateKey()
    local chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    local t = {}
    for i = 1, 7 do
        local idx = math.random(1, #chars)
        t[#t+1] = chars:sub(idx, idx)
    end
    return "SPOOKY-" .. table.concat(t)
end

-- Cria GUI principal
local gui = Instance.new("ScreenGui")
gui.Name = "DeltaKeySystemHalloween"
gui.ResetOnSpawn = false
gui.Parent = Players.LocalPlayer:WaitForChild("PlayerGui")

-- Frame principal (REDUZIDO) - Tema Halloween
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 320, 0, 200)
main.Position = UDim2.new(0.5, -160, 0.5, -100)
main.BackgroundColor3 = Color3.fromRGB(20, 10, 25) -- Roxo escuro
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true
main.Parent = gui

-- Arredondar
local mainCorner = Instance.new("UICorner", main)
mainCorner.CornerRadius = UDim.new(0, 12)

-- Borda laranja pulsante
local glow = Instance.new("UIStroke", main)
glow.Color = Color3.fromRGB(255, 140, 0) -- Laranja Halloween
glow.Thickness = 2
glow.Transparency = 0

-- Título com tema Halloween (MENOR)
local title = Instance.new("Frame", main)
title.Size = UDim2.new(1, 0, 0, 40)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundColor3 = Color3.fromRGB(40, 20, 30) -- Roxo mais claro
title.BorderSizePixel = 0

local titleCorner = Instance.new("UICorner", title)
titleCorner.CornerRadius = UDim.new(0, 10)

local titleLabel = Instance.new("TextLabel", title)
titleLabel.Size = UDim2.new(1, -12, 1, 0)
titleLabel.Position = UDim2.new(0, 6, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "🎃 SPOOKY SCRIPT"
titleLabel.TextColor3 = Color3.fromRGB(255, 165, 0) -- Laranja
titleLabel.Font = Enum.Font.FredokaOne
titleLabel.TextSize = 18
titleLabel.TextXAlignment = Enum.TextXAlignment.Left

-- Gradient Halloween
local titleGradient = Instance.new("UIGradient", title)
titleGradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 140, 0)), -- Laranja
    ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 69, 0))   -- Laranja escuro
}
titleGradient.Rotation = 45

-- Caixa de key (MENOR) - Tema Halloween
local keyBox = Instance.new("TextBox", main)
keyBox.Size = UDim2.new(0.85, 0, 0, 32)
keyBox.Position = UDim2.new(0.075, 0, 0.3, 0)
keyBox.BackgroundColor3 = Color3.fromRGB(30, 15, 35) -- Roxo mais escuro
keyBox.TextColor3 = Color3.fromRGB(255, 200, 150) -- Laranja claro
keyBox.PlaceholderText = "Digite ou cole sua key aqui..."
keyBox.PlaceholderColor3 = Color3.fromRGB(150, 100, 100) -- Vermelho escuro
keyBox.Font = Enum.Font.GothamMedium
keyBox.TextSize = 14

local keyCorner = Instance.new("UICorner", keyBox)
keyCorner.CornerRadius = UDim.new(0, 8)

-- Botões (container) (MENOR)
local btnFrame = Instance.new("Frame", main)
btnFrame.Size = UDim2.new(0.85, 0, 0, 34)
btnFrame.Position = UDim2.new(0.075, 0, 0.55, 0)
btnFrame.BackgroundTransparency = 1

-- Get Key button (esquerda) - Laranja Halloween
local getKeyBtn = Instance.new("TextButton", btnFrame)
getKeyBtn.Size = UDim2.new(0.48, -4, 1, 0)
getKeyBtn.Position = UDim2.new(0, 0, 0, 0)
getKeyBtn.BackgroundColor3 = Color3.fromRGB(255, 140, 0) -- Laranja Halloween
getKeyBtn.Text = "💀 GET KEY"
getKeyBtn.Font = Enum.Font.GothamBold
getKeyBtn.TextSize = 13
getKeyBtn.TextColor3 = Color3.new(0,0,0) -- Texto preto para contraste

local getCorner = Instance.new("UICorner", getKeyBtn)
getCorner.CornerRadius = UDim.new(0, 8)

-- Confirm button (direita) - Verde Halloween
local confirmBtn = Instance.new("TextButton", btnFrame)
confirmBtn.Size = UDim2.new(0.48, -4, 1, 0)
confirmBtn.Position = UDim2.new(0.52, 8, 0, 0)
confirmBtn.BackgroundColor3 = Color3.fromRGB(100, 200, 50) -- Verde Halloween
confirmBtn.Text = "🦇 CONFIRMAR"
confirmBtn.Font = Enum.Font.GothamBold
confirmBtn.TextSize = 13
confirmBtn.TextColor3 = Color3.new(0,0,0) -- Texto preto para contraste

local confirmCorner = Instance.new("UICorner", confirmBtn)
confirmCorner.CornerRadius = UDim.new(0, 8)

-- Label de status (MENOR)
local status = Instance.new("TextLabel", main)
status.Size = UDim2.new(1, 0, 0, 24)
status.Position = UDim2.new(0, 0, 0.78, 0)
status.BackgroundTransparency = 1
status.Text = ""
status.Font = Enum.Font.GothamMedium
status.TextSize = 12
status.TextColor3 = Color3.fromRGB(255, 200, 100) -- Laranja dourado

-- Ícones Halloween (MENORES)
local pumpkinIcon = Instance.new("ImageLabel", main)
pumpkinIcon.Size = UDim2.new(0, 22, 0, 22)
pumpkinIcon.Position = UDim2.new(0.04, 0, 0.15, 0)
pumpkinIcon.BackgroundTransparency = 1
pumpkinIcon.Image = "rbxassetid://7733960981" -- Ícone de abóbora se disponível
pumpkinIcon.ImageColor3 = Color3.fromRGB(255, 140, 0) -- Laranja
pumpkinIcon.ImageTransparency = 0

local batIcon = Instance.new("ImageLabel", main)
batIcon.Size = UDim2.new(0, 20, 0, 20)
batIcon.Position = UDim2.new(0.89, 0, 0.15, 0)
batIcon.BackgroundTransparency = 1
batIcon.Image = "rbxassetid://7733960981" -- Ícone de morcego se disponível
batIcon.ImageColor3 = Color3.fromRGB(100, 200, 50) -- Verde Halloween
batIcon.Rotation = 0
batIcon.ImageTransparency = 0

-- Função de animação de clique Halloween
local function animateClick(btn, normalColor, pressColor)
    local down = TweenService:Create(btn, TweenInfo.new(0.09), {
        BackgroundColor3 = pressColor,
        Size = btn.Size - UDim2.new(0, 4, 0, 2)
    })
    local up = TweenService:Create(btn, TweenInfo.new(0.09), {
        BackgroundColor3 = normalColor,
        Size = btn.Size
    })
    down:Play()
    down.Completed:Wait()
    up:Play()
end

-- Função: abrir site e gerar key (GET KEY)
local function onGetKey()
    animateClick(getKeyBtn, Color3.fromRGB(255,140,0), Color3.fromRGB(255, 100, 0))
    status.Text = "🕷️ Abrindo site..."
    status.TextColor3 = Color3.fromRGB(255, 200, 100)

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
        status.Text = "🦇 Link copiado!"
        status.TextColor3 = Color3.fromRGB(100, 255, 100)
    else
        keyBox.Text = GENERATED_KEY
        status.Text = "💀 Cole o link no navegador"
        status.TextColor3 = Color3.fromRGB(255, 200, 100)
    end
    
    -- anima ícones Halloween
    TweenService:Create(pumpkinIcon, TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {ImageTransparency = 0.2}):Play()
    TweenService:Create(batIcon, TweenInfo.new(0.6, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Rotation = batIcon.Rotation + 180}):Play()
    
    task.delay(2.2, function()
        if setclipboard then
            keyBox.Text = ""
        end
        status.Text = ""
    end)
end

-- Função: confirmar key (agora valida contra GENERATED_KEY)
local function onConfirm()
    animateClick(confirmBtn, Color3.fromRGB(100,200,50), Color3.fromRGB(150, 255, 100))
    local text = tostring(keyBox.Text or "")
    text = text:gsub("^%s*(.-)%s*$", "%1") -- trim

    -- Se não foi gerada nenhuma key ainda, automaticamente falha
    if not GENERATED_KEY then
        status.Text = "💀 Use GET KEY primeiro"
        status.TextColor3 = Color3.fromRGB(255, 100, 100)
        -- shake assustador
        local orig = keyBox.Position
        for i = 1, 4 do
            TweenService:Create(keyBox, TweenInfo.new(0.04, Enum.EasingStyle.Linear), {
                Position = UDim2.new(orig.X.Scale, orig.X.Offset + (i%2==0 and 4 or -4), orig.Y.Scale, orig.Y.Offset)
            }):Play()
            wait(0.04)
        end
        TweenService:Create(keyBox, TweenInfo.new(0.06), {Position = orig}):Play()
        task.delay(1.8, function()
            status.Text = ""
        end)
        return
    end

    -- Se campo vazio => erro
    if text == "" then
        status.Text = "🕸️ Insira a key"
        status.TextColor3 = Color3.fromRGB(255, 100, 100)
        -- shake assustador
        local orig = keyBox.Position
        for i = 1, 4 do
            TweenService:Create(keyBox, TweenInfo.new(0.04, Enum.EasingStyle.Linear), {
                Position = UDim2.new(orig.X.Scale, orig.X.Offset + (i%2==0 and 4 or -4), orig.Y.Scale, orig.Y.Offset)
            }):Play()
            wait(0.04)
        end
        TweenService:Create(keyBox, TweenInfo.new(0.06), {Position = orig}):Play()
        task.delay(1.8, function()
            status.Text = ""
        end)
        return
    end

    -- Agora valida: somente a GENERATED_KEY é aceita
    if text == GENERATED_KEY then
        status.Text = "🎃 Key confirmada!"
        status.TextColor3 = Color3.fromRGB(100, 255, 100)
        -- animações Halloween
        TweenService:Create(pumpkinIcon, TweenInfo.new(0.45, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {ImageTransparency = 0}):Play()
        TweenService:Create(batIcon, TweenInfo.new(0.8, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Rotation = batIcon.Rotation + 360}):Play()
        -- opcional: "consome" a key
        GENERATED_KEY = nil
    else
        -- falha — qualquer outra key dá erro
        status.Text = "👻 Key inválida"
        status.TextColor3 = Color3.fromRGB(255, 100, 100)
        -- shake assustador
        local orig = keyBox.Position
        for i = 1, 4 do
            TweenService:Create(keyBox, TweenInfo.new(0.04, Enum.EasingStyle.Linear), {
                Position = UDim2.new(orig.X.Scale, orig.X.Offset + (i%2==0 and 4 or -4), orig.Y.Scale, orig.Y.Offset)
            }):Play()
            wait(0.04)
        end
        TweenService:Create(keyBox, TweenInfo.new(0.06), {Position = orig}):Play()
    end
    
    task.delay(1.8, function()
        status.Text = ""
        keyBox.Text = ""
    end)
end

-- Eventos
getKeyBtn.MouseButton1Click:Connect(onGetKey)
confirmBtn.MouseButton1Click:Connect(onConfirm)

-- Hover effects Halloween
getKeyBtn.MouseEnter:Connect(function()
    TweenService:Create(getKeyBtn, TweenInfo.new(0.12), {BackgroundColor3 = Color3.fromRGB(255, 165, 0)}):Play()
end)
getKeyBtn.MouseLeave:Connect(function()
    TweenService:Create(getKeyBtn, TweenInfo.new(0.12), {BackgroundColor3 = Color3.fromRGB(255, 140, 0)}):Play()
end)

confirmBtn.MouseEnter:Connect(function()
    TweenService:Create(confirmBtn, TweenInfo.new(0.12), {BackgroundColor3 = Color3.fromRGB(150, 255, 100)}):Play()
end)
confirmBtn.MouseLeave:Connect(function()
    TweenService:Create(confirmBtn, TweenInfo.new(0.12), {BackgroundColor3 = Color3.fromRGB(100, 200, 50)}):Play()
end)

-- Animações contínuas Halloween
spawn(function()
    while true do
        -- glow thickness pulse (efeito fantasma)
        TweenService:Create(glow, TweenInfo.new(1.2), {Thickness = 2.5}):Play()
        wait(0.8)
        TweenService:Create(glow, TweenInfo.new(1.2), {Thickness = 2}):Play()

        -- pumpkin icon pulse
        TweenService:Create(pumpkinIcon, TweenInfo.new(1.6), {ImageTransparency = 0.4}):Play()
        wait(0.9)
        TweenService:Create(pumpkinIcon, TweenInfo.new(1.6), {ImageTransparency = 0}):Play()
        
        -- bat flying animation
        TweenService:Create(batIcon, TweenInfo.new(3.5), {Rotation = batIcon.Rotation + 90}):Play()
        wait(1.2)
    end
end)

print("🎃 Spooky Script Halloween carregado! 💀")
