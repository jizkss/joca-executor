-- Full Executor gui
local TweenService = game:GetService("TweenService")
local HttpService = game:GetService("HttpService")
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "JocaHubExecutorUI"

-- Cores do tema Joca Hub
local theme = {
    background = Color3.fromRGB(20, 25, 35),
    header = Color3.fromRGB(30, 40, 55),
    accent = Color3.fromRGB(0, 100, 255),
    accentLight = Color3.fromRGB(100, 200, 255),
    text = Color3.fromRGB(240, 240, 240),
    secondary = Color3.fromRGB(40, 50, 65),
    button = Color3.fromRGB(0, 70, 255),
    buttonHover = Color3.fromRGB(0, 100, 190)
}

-- === EXECUTOR GUI ===
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 520, 0, 320)
mainFrame.Position = UDim2.new(0.5, -260, 0, -350)
mainFrame.BackgroundColor3 = theme.background
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.BorderSizePixel = 0

local mainCorner = Instance.new("UICorner", mainFrame)
mainCorner.CornerRadius = UDim.new(0, 12)

local header = Instance.new("Frame", mainFrame)
header.Size = UDim2.new(1, 0, 0, 40)
header.BackgroundColor3 = theme.header
header.BorderSizePixel = 0

local headerCorner = Instance.new("UICorner", header)
headerCorner.CornerRadius = UDim.new(0, 12)

local headerGradient = Instance.new("UIGradient", header)
headerGradient.Rotation = 90
headerGradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, theme.header),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(25, 35, 45))
})

TweenService:Create(mainFrame, TweenInfo.new(0.6, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
    Position = UDim2.new(0.5, -260, 0.5, -160)
}):Play()

-- Logo do Joca Hub com peixe
local logoFrame = Instance.new("Frame", header)
logoFrame.Size = UDim2.new(0, 36, 0, 36)
logoFrame.Position = UDim2.new(0, 10, 0, 2)
logoFrame.BackgroundColor3 = theme.accent
logoFrame.BorderSizePixel = 0

local logoCorner = Instance.new("UICorner", logoFrame)
logoCorner.CornerRadius = UDim.new(0, 8)

local peixeLabel = Instance.new("TextLabel", logoFrame)
peixeLabel.Size = UDim2.new(1, 0, 1, 0)
peixeLabel.Text = "￰ﾟﾐﾟ"
peixeLabel.Font = Enum.Font.GothamBold
peixeLabel.TextSize = 22
peixeLabel.TextColor3 = theme.text
peixeLabel.BackgroundTransparency = 1

local title = Instance.new("TextLabel", header)
title.Size = UDim2.new(1, -50, 1, 0)
title.Position = UDim2.new(0, 50, 0, 0)
title.Text = "JOCA HUB EXECUTOR"
title.Font = Enum.Font.GothamBold
title.TextColor3 = theme.text
title.TextSize = 16
title.TextXAlignment = Enum.TextXAlignment.Left
title.BackgroundTransparency = 1

local subtitle = Instance.new("TextLabel", header)
subtitle.Size = UDim2.new(1, -50, 0, 14)
subtitle.Position = UDim2.new(0, 50, 0, 22)
subtitle.Text = "Backdoor Executor v2.0"
subtitle.Font = Enum.Font.Gotham
subtitle.TextColor3 = theme.accentLight
subtitle.TextSize = 11
subtitle.TextXAlignment = Enum.TextXAlignment.Left
subtitle.BackgroundTransparency = 1

local inputBg = Instance.new("Frame", mainFrame)
inputBg.Size = UDim2.new(1, -24, 0, 180)
inputBg.Position = UDim2.new(0, 12, 0, 50)
inputBg.BackgroundColor3 = theme.secondary
inputBg.BorderSizePixel = 0

local inputCorner = Instance.new("UICorner", inputBg)
inputCorner.CornerRadius = UDim.new(0, 8)

local input = Instance.new("TextBox", inputBg)
input.Size = UDim2.new(1, -16, 1, -16)
input.Position = UDim2.new(0, 8, 0, 8)
input.Text = "-- Type require(123456)() or any command\n-- Joca Hub Executor - Secure & Fast"
input.ClearTextOnFocus = false
input.TextWrapped = true
input.MultiLine = true
input.TextYAlignment = Enum.TextYAlignment.Top
input.TextXAlignment = Enum.TextXAlignment.Left
input.Font = Enum.Font.Code
input.TextSize = 14
input.TextColor3 = theme.text
input.BackgroundTransparency = 1
input.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)

local closeBtn = Instance.new("TextButton", header)
closeBtn.Size = UDim2.new(0, 26, 0, 26)
closeBtn.Position = UDim2.new(1, -30, 0, 7)
closeBtn.Text = "ￃﾗ"
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 18
closeBtn.TextColor3 = theme.text
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
closeBtn.BorderSizePixel = 0

local closeCorner = Instance.new("UICorner", closeBtn)
closeCorner.CornerRadius = UDim.new(1, 0)

local openBtn = Instance.new("TextButton", gui)
openBtn.Size = UDim2.new(0, 40, 0, 40)
openBtn.Position = UDim2.new(0, 20, 0, 100)
openBtn.Text = "￰ﾟﾐﾟ"
openBtn.Font = Enum.Font.GothamBold
openBtn.TextSize = 28
openBtn.TextColor3 = theme.text
openBtn.BackgroundColor3 = theme.accent
openBtn.Active = true
openBtn.Draggable = true
openBtn.BorderSizePixel = 0

local openCorner = Instance.new("UICorner", openBtn)
openCorner.CornerRadius = UDim.new(1, 0)

local openGradient = Instance.new("UIGradient", openBtn)
openGradient.Rotation = 45
openGradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, theme.accent),
    ColorSequenceKeypoint.new(1, theme.accentLight)
})

openBtn.Visible = false

-- Funￃﾧￃﾣo para criar botￃﾵes estilizados
local function createStyledButton(parent, size, position, text)
    local button = Instance.new("TextButton", parent)
    button.Size = size
    button.Position = position
    button.Text = text
    button.Font = Enum.Font.GothamBold
    button.TextSize = 14
    button.TextColor3 = theme.text
    button.BackgroundColor3 = theme.button
    button.BorderSizePixel = 0
    button.AutoButtonColor = false
    
    local corner = Instance.new("UICorner", button)
    corner.CornerRadius = UDim.new(0, 8)
    
    local gradient = Instance.new("UIGradient", button)
    gradient.Rotation = 90
    gradient.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, theme.button),
        ColorSequenceKeypoint.new(1, theme.buttonHover)
    })
    
    -- Efeito hover
    button.MouseEnter:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {
            BackgroundColor3 = theme.buttonHover
        }):Play()
    end)
    
    button.MouseLeave:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {
            BackgroundColor3 = theme.button
        }):Play()
    end)
    
    return button
end

-- Botￃﾵes posicionados mais para cima (Y position alterada de -50 para -60)
local execBtn = createStyledButton(mainFrame, UDim2.new(0, 240, 0, 40), UDim2.new(0, 11, 1, -80), "EXECUTAR NO SERVIDOR")
local clearBtn = createStyledButton(mainFrame, UDim2.new(0, 240, 0, 40), UDim2.new(0, 265, 1, -80), "LIMPAR CￃﾓDIGO")

-- Footer
local footer = Instance.new("Frame", mainFrame)
footer.Size = UDim2.new(1, 0, 0, 25)
footer.Position = UDim2.new(0, 0, 1, -25)
footer.BackgroundColor3 = theme.header
footer.BorderSizePixel = 0

local footerCorner = Instance.new("UICorner", footer)
footerCorner.CornerRadius = UDim.new(0, 0, 0, 12)

local statusLabel = Instance.new("TextLabel", footer)
statusLabel.Size = UDim2.new(1, -20, 1, 0)
statusLabel.Position = UDim2.new(0, 10, 0, 0)
statusLabel.Text = "Joca Hub Executor - Pronto para executar"
statusLabel.Font = Enum.Font.Gotham
statusLabel.TextColor3 = theme.accentLight
statusLabel.TextSize = 11
statusLabel.TextXAlignment = Enum.TextXAlignment.Left
statusLabel.BackgroundTransparency = 1

execBtn.MouseButton1Click:Connect(function()
    local code = input.Text
    statusLabel.Text = "Executando cￃﾳdigo no servidor"
    
    for _, obj in ipairs(game:GetService("ReplicatedStorage"):GetDescendants()) do
        if obj:IsA("RemoteEvent") then
            pcall(function() obj:FireServer(code) end)
        end
    end
    
    task.wait(1)
    statusLabel.Text = "Cￃﾳdigo executado com sucesso"
end)

clearBtn.MouseButton1Click:Connect(function()
    input.Text = ""
    statusLabel.Text = "Cￃﾳdigo limpo"
end)

closeBtn.MouseButton1Click:Connect(function()
    TweenService:Create(mainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.In), {
        Position = UDim2.new(0.5, -260, 0, -350)
    }):Play()
    task.wait(0.4)
    mainFrame.Visible = false
    openBtn.Visible = true
end)

openBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    openBtn.Visible = false
    mainFrame.Position = UDim2.new(0.5, -260, 0, -350)
    TweenService:Create(mainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Position = UDim2.new(0.5, -260, 0.5, -160)
    }):Play()
end)

-- Efeitos de sombra
local shadow = Instance.new("ImageLabel", mainFrame)
shadow.BackgroundTransparency = 1
shadow.Size = UDim2.new(1, 24, 1, 24)
shadow.Position = UDim2.new(0, -12, 0, -12)
shadow.Image = "rbxassetid://1316045217"
shadow.ImageColor3 = Color3.new(0, 0, 0)
shadow.ImageTransparency = 0.9
shadow.ScaleType = Enum.ScaleType.Slice
shadow.SliceCenter = Rect.new(10, 10, 118, 118)
shadow.ZIndex = -1
