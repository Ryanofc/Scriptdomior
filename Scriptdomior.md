-- LocalScript (StarterPlayerScripts)

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

local normalSpeed = 16
local runSpeed = 50
local running = false

local correctKey = "17152025" -- SUA KEY FIXA

-- Função para criar a GUI principal (Main)
local function createMainGui()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "OMiorGui"
    screenGui.Parent = player:WaitForChild("PlayerGui")
    
    local titleLabel = Instance.new("TextLabel")
    titleLabel.Name = "TitleLabel"
    titleLabel.Parent = screenGui
    titleLabel.Size = UDim2.new(0, 300, 0, 50)
    titleLabel.Position = UDim2.new(0.5, -150, 0, 20)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = "O Mior"
    titleLabel.Font = Enum.Font.Arcade
    titleLabel.TextScaled = true
    titleLabel.TextColor3 = Color3.new(1, 1, 1)

    local runButton = Instance.new("TextButton")
    runButton.Name = "RunButton"
    runButton.Parent = screenGui
    runButton.Size = UDim2.new(0, 200, 0, 50)
    runButton.Position = UDim2.new(0.5, -100, 0, 100)
    runButton.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
    runButton.Text = "Correr Mais Rápido"
    runButton.Font = Enum.Font.GothamBlack
    runButton.TextScaled = true
    runButton.TextColor3 = Color3.new(1, 1, 1)

    local teleportButton = Instance.new("TextButton")
    teleportButton.Name = "TeleportButton"
    teleportButton.Parent = screenGui
    teleportButton.Size = UDim2.new(0, 200, 0, 50)
    teleportButton.Position = UDim2.new(0.5, -100, 0, 170)
    teleportButton.BackgroundColor3 = Color3.fromRGB(255, 100, 0)
    teleportButton.Text = "Teleportar para Bola"
    teleportButton.Font = Enum.Font.GothamBlack
    teleportButton.TextScaled = true
    teleportButton.TextColor3 = Color3.new(1, 1, 1)

    local toggleButton = Instance.new("TextButton")
    toggleButton.Name = "ToggleButton"
    toggleButton.Parent = screenGui
    toggleButton.Size = UDim2.new(0, 50, 0, 50)
    toggleButton.Position = UDim2.new(0, 10, 0.5, -25)
    toggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    toggleButton.Text = "GUI"
    toggleButton.Font = Enum.Font.GothamBlack
    toggleButton.TextScaled = true
    toggleButton.TextColor3 = Color3.new(1, 1, 1)

    local guiElements = {titleLabel, runButton, teleportButton}
    local guiVisible = true

    toggleButton.MouseButton1Click:Connect(function()
        guiVisible = not guiVisible
        for _, element in pairs(guiElements) do
            element.Visible = guiVisible
        end
    end)

    runButton.MouseButton1Click:Connect(function()
        if not running then
            humanoid.WalkSpeed = runSpeed
            runButton.Text = "Parar de Correr"
            running = true
        else
            humanoid.WalkSpeed = normalSpeed
            runButton.Text = "Correr Mais Rápido"
            running = false
        end
    end)

    teleportButton.MouseButton1Click:Connect(function()
        local ball = workspace:FindFirstChild("Ball")
        if ball and character and character:FindFirstChild("HumanoidRootPart") then
            character.HumanoidRootPart.CFrame = ball.CFrame * CFrame.new(0, 3, 0)
        end
    end)

    -- A GUI começa invisível
    for _, element in pairs(guiElements) do
        element.Visible = false
    end
end

-- Função para criar a GUI da Key
local function createKeyGui()
    local keyGui = Instance.new("ScreenGui")
    keyGui.Name = "KeyGui"
    keyGui.Parent = player:WaitForChild("PlayerGui")
    
    local frame = Instance.new("Frame")
    frame.Parent = keyGui
    frame.Size = UDim2.new(0, 320, 0, 200)
    frame.Position = UDim2.new(0.5, -160, 0.5, -100)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    frame.BorderSizePixel = 0

    local textBox = Instance.new("TextBox")
    textBox.Parent = frame
    textBox.Size = UDim2.new(0, 280, 0, 50)
    textBox.Position = UDim2.new(0.5, -140, 0, 20)
    textBox.PlaceholderText = "Digite a Key"
    textBox.Font = Enum.Font.Gotham
    textBox.TextScaled = true
    textBox.TextColor3 = Color3.new(1, 1, 1)
    textBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

    local copyButton = Instance.new("TextButton")
    copyButton.Parent = frame
    copyButton.Size = UDim2.new(0, 130, 0, 40)
    copyButton.Position = UDim2.new(0, 10, 0, 90)
    copyButton.Text = "Copiar o Link"
    copyButton.Font = Enum.Font.GothamBlack
    copyButton.TextScaled = true
    copyButton.TextColor3 = Color3.new(1, 1, 1)
    copyButton.BackgroundColor3 = Color3.fromRGB(0, 150, 255)

    local confirmButton = Instance.new("TextButton")
    confirmButton.Parent = frame
    confirmButton.Size = UDim2.new(0, 130, 0, 40)
    confirmButton.Position = UDim2.new(0, 180, 0, 90)
    confirmButton.Text = "Confirmar"
    confirmButton.Font = Enum.Font.GothamBlack
    confirmButton.TextScaled = true
    confirmButton.TextColor3 = Color3.new(1, 1, 1)
    confirmButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)

    -- Função copiar link
    copyButton.MouseButton1Click:Connect(function()
        setclipboard("https://linktr.ee/ryangamer7")
    end)

    -- Função confirmar Key
    confirmButton.MouseButton1Click:Connect(function()
        if textBox.Text == correctKey then
            keyGui:Destroy()
            createMainGui()
        else
            textBox.Text = ""
            textBox.PlaceholderText = "Key Errada! Pegue no Link!"
        end
    end)
end

-- Quando o script começar:
createKeyGui()
