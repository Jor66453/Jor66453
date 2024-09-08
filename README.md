-- Load Rayfield library
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Create the main window
local Window = Rayfield:CreateWindow({
    Name = "SURVIVE THE SLASHER HUB🔪",
    LoadingTitle = "Survive The Slasher",
    LoadingSubtitle = "By Jor",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "SURVIVE THE SLASHER HUB🔪"
    },
    Discord = {
        Enabled = true,
        Invite = "9WsJWq4E8b",
        RememberJoins = true 
    },
    KeySystem = true,  -- Enable the key system
    KeySettings = {
        Title = "Key System🔑",
        Subtitle = "Please Enter A Key To Access The Script!🔪🔑",
        Note = "The Key is in the discord🔑🤖",
        FileName = "Key",
        SaveKey = true,
        Key = {"PlayBoxCHE3ATI#GSurviveTheSlasherKey34649583"}  -- Set your desired key here
    }
})

-- Create tabs
local MainTab = Window:CreateTab("Main", nil)
local SurviTab = Window:CreateTab("Survivor", nil)
local SlrTab = Window:CreateTab("Slasher", nil)
local ExtrTab = Window:CreateTab("Extra", nil)
local CrdTab = Window:CreateTab("Credits", nil)

-- Show a notification
Rayfield:Notify({
    Title = "Success!",
    Content = "Welcome!",
    Duration = 6.5,
    Image = nil,
    Actions = {
        Ignore = {
            Name = "Dismiss",
            Callback = function() end
        }
    }
})

local infiniteJumpEnabled = false

game:GetService("UserInputService").JumpRequest:Connect(function()
    if infiniteJumpEnabled then
        game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)

local staffProtectionToggle = false
local staffProtectionConnection
local localPlayer = game.Players.LocalPlayer
local textLabel -- Variable to keep track of the activation label
local warningLabel -- Variable to keep track of the deactivation warning label

-- Main toggle for staff protection
MainTab:CreateToggle({
    Name = "Staff Protection🛡️",
    CurrentValue = false,
    Callback = function(state)
        staffProtectionToggle = state

-- Function to create a styled activation TextLabel
local function createStyledTextLabel()
    local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
    textLabel = Instance.new("TextLabel", screenGui)
    textLabel.Size = UDim2.new(0, 400, 0, 100)
    textLabel.Position = UDim2.new(0.5, -200, -0.2, 0) -- Start off-screen above the screen
    textLabel.Text = "Activated! You will now be kicked out of the game if an Admin/Developer Joins."
    textLabel.TextScaled = true
    textLabel.BackgroundTransparency = 1
    textLabel.TextColor3 = Color3.fromRGB(50, 205, 50) -- Lighter green
    textLabel.Font = Enum.Font.GothamBlack
    textLabel.TextStrokeTransparency = 0
    textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

    -- Slide-in effect
    local tweenService = game:GetService("TweenService")
    local targetPosition = UDim2.new(0.5, -200, 0.5, -50) -- Centered position
    local slideInTween = tweenService:Create(textLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = targetPosition})
    slideInTween:Play()

    -- Smooth fade-out
    task.delay(4, function()
        local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
        fadeOutTween:Play()
        fadeOutTween.Completed:Connect(function()
            textLabel:Destroy()
        end)
    end)
end

-- Function to create a styled warning TextLabel
local function createWarningTextLabel()
    local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
    warningLabel = Instance.new("TextLabel", screenGui)
    warningLabel.Size = UDim2.new(0, 400, 0, 100)
    warningLabel.Position = UDim2.new(0.5, -200, -0.2, 0) -- Start off-screen above the screen
    warningLabel.Text = "Deactivated. DANGER, It's recommended to leave this on!!"
    warningLabel.TextScaled = true
    warningLabel.BackgroundTransparency = 1
    warningLabel.TextColor3 = Color3.fromRGB(227, 0, 34) -- Lighter red
    warningLabel.Font = Enum.Font.GothamBlack
    warningLabel.TextStrokeTransparency = 0
    warningLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

    -- Slide-in effect
    local tweenService = game:GetService("TweenService")
    local targetPosition = UDim2.new(0.5, -200, 0.5, -50) -- Centered position
    local slideInTween = tweenService:Create(warningLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = targetPosition})
    slideInTween:Play()

    -- Smooth fade-out
    task.delay(4, function()
        local fadeOutTween = tweenService:Create(warningLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
        fadeOutTween:Play()
        fadeOutTween.Completed:Connect(function()
            warningLabel:Destroy()
        end)
    end)
end

        if staffProtectionToggle then
            if not textLabel then
                createStyledTextLabel()
            end

            if warningLabel then
                warningLabel:Destroy() -- Destroy the warning label
                warningLabel = nil -- Reset the warningLabel variable
            end

            local targetUserIDs = {
                [362956857] = true,
                [28724905] = true,
                [707723783] = true,
                [331273890] = true,
                [4791037402] = true,
                [311314197] = true,
                [2241157448] = true,
                [3524879643] = true,
                [862638016] = true,
                [316330352] = true,
                [2977642950] = true,
                [3120444159] = true,
                [3047319330] = true
            }

            local function kickPlayerIfNeeded(joinedPlayer)
                if targetUserIDs[joinedPlayer.UserId] then
                    localPlayer:Kick("A Developer/Moderator Of This Game Has Joined Your Server, You Have Been Kicked For Your Safety.")
                end
            end

            -- Check existing players
            for _, existingPlayer in ipairs(game.Players:GetPlayers()) do
                kickPlayerIfNeeded(existingPlayer)
            end

            -- Check new players
            staffProtectionConnection = game.Players.PlayerAdded:Connect(kickPlayerIfNeeded)

        else
            if textLabel then
                textLabel:Destroy() -- Destroy the activation label
                textLabel = nil -- Reset the textLabel variable
            end

            if not warningLabel then
                createWarningTextLabel()
            end

            if staffProtectionConnection then
                staffProtectionConnection:Disconnect()
                staffProtectionConnection = nil
            end
        end
    end
})

MainTab:CreateButton({
    Name = "Infinite yield❔",
    Callback = function()
        -- Load and execute Infinite Yield
        loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()

        -- Create a ScreenGui and TextLabel
        local player = game.Players.LocalPlayer
        local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
        local textLabel = Instance.new("TextLabel", screenGui)
        textLabel.Size = UDim2.new(0, 400, 0, 100)
        textLabel.Position = UDim2.new(0.5, -200, -0.3, 0) -- Start off-screen at the top
        textLabel.Text = "Infinite yield Executed!"
        textLabel.TextScaled = true
        textLabel.BackgroundTransparency = 1
        textLabel.TextColor3 = Color3.fromRGB(208, 114, 255) -- Lighter, llamative purple
        textLabel.Font = Enum.Font.GothamBlack
        textLabel.TextStrokeTransparency = 0
        textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

        -- Slide-in effect from the top
        local tweenService = game:GetService("TweenService")
        local targetPosition = UDim2.new(0.5, -200, 0.5, -50) -- Centered position
        local slideInTween = tweenService:Create(textLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = targetPosition})
        slideInTween:Play()

        -- Smooth fade-out
        slideInTween.Completed:Connect(function()
            task.delay(4, function()
                local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                fadeOutTween:Play()
                fadeOutTween.Completed:Connect(function()
                    textLabel:Destroy()
                end)
            end)
        end)
    end
})

local infjToggle = false
local localPlayer = game.Players.LocalPlayer
local textLabel -- Variable to keep track of the activation label
local warningLabel -- Variable to keep track of the deactivation warning label
local infiniteJumpEnabled = false -- Variable to control infinite jump

local infJump = MainTab:CreateToggle({
    Name = "Infinite Jump⏫",
    Default = false,
    Callback = function(state)
        infjToggle = state
        infiniteJumpEnabled = infjToggle -- Set the infiniteJumpEnabled based on the toggle state

        -- Function to create a styled activation TextLabel
        local function createStyledTextLabel()
            local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
            textLabel = Instance.new("TextLabel", screenGui)
            textLabel.Size = UDim2.new(0, 400, 0, 100)
            textLabel.Position = UDim2.new(0.5, -200, -0.2, 0) -- Start off-screen above the screen
            textLabel.Text = "Inf Jump ON!"
            textLabel.TextScaled = true
            textLabel.BackgroundTransparency = 1
            textLabel.TextColor3 = Color3.fromRGB(50, 205, 50) -- Lighter green
            textLabel.Font = Enum.Font.GothamBlack
            textLabel.TextStrokeTransparency = 0
            textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

            -- Slide-in effect
            local tweenService = game:GetService("TweenService")
            local targetPosition = UDim2.new(0.5, -200, 0.5, -50) -- Centered position
            local slideInTween = tweenService:Create(textLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = targetPosition})
            slideInTween:Play()

            -- Smooth fade-out
            task.delay(4, function()
                local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                fadeOutTween:Play()
                fadeOutTween.Completed:Connect(function()
                    textLabel:Destroy()
                end)
            end)
        end

        -- Function to create a styled warning TextLabel
        local function createWarningTextLabel()
            local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
            warningLabel = Instance.new("TextLabel", screenGui)
            warningLabel.Size = UDim2.new(0, 400, 0, 100)
            warningLabel.Position = UDim2.new(0.5, -200, -0.2, 0) -- Start off-screen above the screen
            warningLabel.Text = "Inf Jump OFF!"
            warningLabel.TextScaled = true
            warningLabel.BackgroundTransparency = 1
            warningLabel.TextColor3 = Color3.fromRGB(227, 0, 34) -- Lighter red
            warningLabel.Font = Enum.Font.GothamBlack
            warningLabel.TextStrokeTransparency = 0
            warningLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

            -- Slide-in effect
            local tweenService = game:GetService("TweenService")
            local targetPosition = UDim2.new(0.5, -200, 0.5, -50) -- Centered position
            local slideInTween = tweenService:Create(warningLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = targetPosition})
            slideInTween:Play()

            -- Smooth fade-out
            task.delay(4, function()
                local fadeOutTween = tweenService:Create(warningLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                fadeOutTween:Play()
                fadeOutTween.Completed:Connect(function()
                    warningLabel:Destroy()
                end)
            end)
        end

        if infjToggle then
            if not textLabel then
                createStyledTextLabel()
            end

            if warningLabel then
                warningLabel:Destroy() -- Destroy the warning label
                warningLabel = nil -- Reset the warningLabel variable
            end
        else
            if not warningLabel then
                createWarningTextLabel()
            end

            if textLabel then
                textLabel:Destroy() -- Destroy the activation label
                textLabel = nil -- Reset the textLabel variable
            end
        end
    end
})

-- Handle infinite jump functionality
game:GetService("UserInputService").JumpRequest:Connect(function()
    if infiniteJumpEnabled then
        local humanoid = localPlayer.Character and localPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Physics)
            humanoid:ChangeState(Enum.HumanoidStateType.Seated)
            humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)



local function resetTabs(tab)
    for _, button in ipairs(tab:GetChildren()) do
        if button:IsA("GuiButton") then
            button:Destroy()
        end
    end
end

local function handleSpeedChange(speed)
    local player = game.Players.LocalPlayer
    local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")

    if humanoid then
        if speed == 20 then
            resetTabs(SurviTab)
            Rayfield:Notify({
                Title = "Warning",
                Content = "You cannot use Survivor scripts while being slasher!",
                Duration = 5,
                Image = nil
            })
        elseif speed == 16 then
            resetTabs(SlrTab)
            Rayfield:Notify({
                Title = "Warning",
                Content = "You cannot use Slasher scripts while being a survivor!",
                Duration = 5,
                Image = nil
            })
        end
    end
end

local walkSpeedSlider = MainTab:CreateSlider({
    Name = "WalkSpeed🏃‍♀️",
    Range = {16, 500},
    Increment = 1,
    Suffix = "Speed",
    CurrentValue = 16,
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
        handleSpeedChange(Value)
    end
})

local jumpPowerSlider = MainTab:CreateSlider({
    Name = "JumpPower🔼",
    Range = {50, 550},
    Increment = 1,
    Suffix = "JumpPower",
    CurrentValue = 50,
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
    end
})


MainTab:CreateButton({
    Name = "Reset Speed🏃‍♀️🔄️",
    Callback = function()
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16
        infiniteJumpEnabled = false
        walkSpeedSlider:Set(16)

        local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
        local textLabel = Instance.new("TextLabel", screenGui)
        textLabel.Size = UDim2.new(0, 400, 0, 100)
        textLabel.Position = UDim2.new(-0.5, 0, 0.1, 0) -- Start off-screen to the left
        textLabel.Text = "Success! Current Speed Has Been Reset"
        textLabel.TextScaled = true
        textLabel.BackgroundTransparency = 1
        textLabel.TextColor3 = Color3.fromRGB(204, 0, 0) -- Lighter, llamative purple
        textLabel.Font = Enum.Font.GothamBlack
        textLabel.TextStrokeTransparency = 0
        textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

        -- Slide-in effect
        local targetPosition = UDim2.new(0.5, -200, 0.1, 0) -- Final centered position
        game:GetService("TweenService"):Create(textLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = targetPosition}):Play()

        -- Smooth fade out effect after 4 seconds
        task.delay(4, function()
            for i = 0, 1, 0.05 do
                textLabel.TextTransparency = i
                textLabel.TextStrokeTransparency = i
                task.wait(0.05)
            end
            textLabel:Destroy()
        end)
    end
})

MainTab:CreateButton({
    Name = "Reset JumpPower🔼🔄️",
    Callback = function()
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = 50
        infiniteJumpEnabled = false
        jumpPowerSlider:Set(50)

        local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
        local textLabel = Instance.new("TextLabel", screenGui)
        textLabel.Size = UDim2.new(0, 400, 0, 100)
        textLabel.Position = UDim2.new(-0.5, 0, 0.1, 0) -- Start off-screen to the left
        textLabel.Text = "Success! Current JumpPower has been Reset"
        textLabel.TextScaled = true
        textLabel.BackgroundTransparency = 1
        textLabel.TextColor3 = Color3.fromRGB(204, 0, 0) -- Lighter, llamative purple
        textLabel.Font = Enum.Font.GothamBlack
        textLabel.TextStrokeTransparency = 0
        textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

        -- Slide-in effect
        local targetPosition = UDim2.new(0.5, -200, 0.1, 0) -- Final centered position
        game:GetService("TweenService"):Create(textLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = targetPosition}):Play()

        -- Smooth fade out effect after 4 seconds
        task.delay(4, function()
            for i = 0, 1, 0.05 do
                textLabel.TextTransparency = i
                textLabel.TextStrokeTransparency = i
                task.wait(0.05)
            end
            textLabel:Destroy()
        end)
    end
})

MainTab:CreateToggle({
    Name = "Spy Private Messages👁️",
    Default = false,
    Callback = function(value)
        enabled = value --chat "/spy" to toggle!
        spyOnMyself = true --if true will check your messages too
        public = false --if true will chat the logs publicly (fun, risky)
        publicItalics = true --if true will use /me to stand out
        privateProperties = {
            Color = Color3.fromRGB(93, 138, 168), 
            Font = Enum.Font.Creepster,
            TextSize = 24
        }

        local StarterGui = game:GetService("StarterGui")
        local Players = game:GetService("Players")
        local player = Players.LocalPlayer or Players:GetPropertyChangedSignal("LocalPlayer"):Wait() or Players.LocalPlayer
        local saymsg = game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("SayMessageRequest")
        local getmsg = game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("OnMessageDoneFiltering")
        local instance = (_G.chatSpyInstance or 0) + 1
        _G.chatSpyInstance = instance

        local function onChatted(p, msg)
            if _G.chatSpyInstance == instance then
                if p == player and msg:lower():sub(1, 4) == "/spy" then
                    enabled = not enabled
                    wait(0.3)
                    privateProperties.Text = "[SPY " .. (enabled and "EN" or "DIS") .. "ABLED]"
                    StarterGui:SetCore("ChatMakeSystemMessage", privateProperties)
                elseif enabled and (spyOnMyself == true or p ~= player) then
                    msg = msg:gsub("[\n\r]", ''):gsub("\t", ' '):gsub("[ ]+", ' ')
                    local hidden = true
                    local conn = getmsg.OnClientEvent:Connect(function(packet, channel)
                        if packet.SpeakerUserId == p.UserId and packet.Message == msg:sub(#msg - #packet.Message + 1) and (channel == "All" or (channel == "Team" and public == false and Players[packet.FromSpeaker].Team == player.Team)) then
                            hidden = false
                        end
                    end)
                    wait(1)
                    conn:Disconnect()
                    if hidden and enabled then
                        if public then
                            saymsg:FireServer((publicItalics and "/me " or '') .. "[SPY] [" .. p.Name .. "]: " .. msg, "All")
                        else
                            privateProperties.Text = "[SPY] [" .. p.Name .. "]: " .. msg
                            StarterGui:SetCore("ChatMakeSystemMessage", privateProperties)
                        end
                    end
                end
            end
        end

        for _, p in ipairs(Players:GetPlayers()) do
            p.Chatted:Connect(function(msg) onChatted(p, msg) end)
        end
        Players.PlayerAdded:Connect(function(p)
            p.Chatted:Connect(function(msg) onChatted(p, msg) end)
        end)
        privateProperties.Text = "[SPY " .. (enabled and "EN" or "DIS") .. "ABLED]"
        StarterGui:SetCore("ChatMakeSystemMessage", privateProperties)
        if not player.PlayerGui:FindFirstChild("Chat") then wait(3) end
        local chatFrame = player.PlayerGui.Chat.Frame
        chatFrame.ChatChannelParentFrame.Visible = true
        chatFrame.ChatBarParentFrame.Position = chatFrame.ChatChannelParentFrame.Position + UDim2.new(0, 0, chatFrame.ChatChannelParentFrame.Size.Y.Scale, chatFrame.ChatChannelParentFrame.Size.Y.Offset)
    end
})

local espToggle = false
local espConnection
local activatedLabel
local deactivatedLabel
local warningLabel

SurviTab:CreateToggle({
    Name = "ESP👁️",
    CurrentValue = false,
    Callback = function(state)
        local player = game:GetService("Players").LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")

        local function createStyledTextLabel(text, color)
            local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
            local textLabel = Instance.new("TextLabel", screenGui)
            textLabel.Size = UDim2.new(0, 600, 0, 100)
            textLabel.Position = UDim2.new(0.5, -300, -0.1, 0)
            textLabel.Text = text
            textLabel.TextScaled = true
            textLabel.BackgroundTransparency = 1
            textLabel.TextColor3 = color
            textLabel.Font = Enum.Font.GothamBlack
            textLabel.TextStrokeTransparency = 0
            textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)

            local tweenService = game:GetService("TweenService")
            local targetPosition = UDim2.new(0.5, -300, 0.5, -50)
            local slideInTween = tweenService:Create(textLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = targetPosition})
            slideInTween:Play()

            slideInTween.Completed:Connect(function()
                task.delay(4, function()
                    local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                    fadeOutTween:Play()
                    fadeOutTween.Completed:Connect(function()
                        textLabel:Destroy()
                    end)
                end)
            end)

            return textLabel
        end

        espToggle = state

        if espToggle then
            if warningLabel then
                warningLabel:Destroy()
            end
            activatedLabel = createStyledTextLabel("ESP has been activated.", Color3.fromRGB(50, 205, 50))

            local Players = game:GetService("Players")
            local RunService = game:GetService("RunService")
            local Camera = game:GetService("Workspace").CurrentCamera
            local LocalPlayer = Players.LocalPlayer

            local function createBillboard(player)
                local character = player.Character
                if character and character:FindFirstChild("Head") then
                    local head = character.Head
                    local billboard = Instance.new("BillboardGui", head)
                    billboard.Name = "ESP"
                    billboard.Size = UDim2.new(4, 0, 1.5, 0)
                    billboard.Adornee = head
                    billboard.AlwaysOnTop = true
                    billboard.StudsOffset = Vector3.new(0, 2, 0)

                    local nameLabel = Instance.new("TextLabel", billboard)
                    nameLabel.Size = UDim2.new(1, 0, 1, 0)
                    nameLabel.BackgroundTransparency = 0
                    nameLabel.BackgroundColor3 = Color3.fromRGB(64, 64, 64)
                    nameLabel.Text = player.Name
                    nameLabel.TextScaled = true
                    nameLabel.Font = Enum.Font.SourceSans
                    nameLabel.TextColor3 = Color3.new(1, 1, 1)
                    nameLabel.BorderSizePixel = 0
                    nameLabel.TextStrokeTransparency = 0.5
                    nameLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
                    nameLabel.ZIndex = 2

                    local border = Instance.new("Frame", nameLabel)
                    border.Size = UDim2.new(1, 4, 1, 4)
                    border.Position = UDim2.new(0, -2, 0, -2)
                    border.BackgroundColor3 = Color3.fromRGB(169, 169, 169)
                    border.BorderSizePixel = 0
                    border.ZIndex = 1

                    local corner = Instance.new("UICorner", nameLabel)
                    corner.CornerRadius = UDim.new(0, 20)

                    local borderCorner = Instance.new("UICorner", border)
                    borderCorner.CornerRadius = UDim.new(0, 20)
                end
            end

            local function createBox(character)
                local parts = {"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"}
                for _, partName in pairs(parts) do
                    local part = character:FindFirstChild(partName)
                    if part then
                        local box = Instance.new("BoxHandleAdornment")
                        box.Name = "ESPBox"
                        box.Adornee = part
                        box.Size = part.Size + Vector3.new(0.2, 0.2, 0.2)
                        box.Color3 = Color3.new(1, 1, 1)
                        box.Transparency = 1
                        box.AlwaysOnTop = true
                        box.ZIndex = 10
                        box.Parent = part
                    end
                end
            end

            local function isPartVisible(part)
                local camera = Camera
                local partPos = part.Position
                local _, onScreen = camera:WorldToViewportPoint(partPos)

                if onScreen then
                    local ray = Ray.new(camera.CFrame.Position, (partPos - camera.CFrame.Position).unit * 500)
                    local hitPart = workspace:FindPartOnRay(ray, workspace)
                    return hitPart and hitPart:IsDescendantOf(part.Parent)
                end
                return false
            end

            local function hasSpeedShake(player)
                local backpack = player.Backpack
                local character = player.Character
                return backpack:FindFirstChild("Speed Shake") or character:FindFirstChild("Speed Shake")
            end

            local function updateESP()
                for _, player in pairs(Players:GetPlayers()) do
                    if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Humanoid") then
                        local humanoid = player.Character.Humanoid
                        local walkSpeed = humanoid.WalkSpeed
                        local head = player.Character:FindFirstChild("Head")
                        local esp = head and head:FindFirstChild("ESP")

                        if not esp then
                            createBillboard(player)
                            esp = head:FindFirstChild("ESP")
                        end

                        for _, partName in pairs({"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"}) do
                            local part = player.Character:FindFirstChild(partName)
                            if part and not part:FindFirstChild("ESPBox") then
                                createBox(player.Character)
                            end
                        end

                        local color
                        if hasSpeedShake(player) then
                            if walkSpeed ~= 5 and walkSpeed ~= 16 and walkSpeed ~= 24 and walkSpeed ~= 25 then
                                color = Color3.fromRGB(252, 76, 2)
                            else
                                color = Color3.fromRGB(255, 255, 255)
                            end
                        else
                            if walkSpeed == 16 then
                                color = Color3.fromRGB(50, 205, 50)
                            elseif walkSpeed == 5 then
                                color = Color3.fromRGB(204, 204, 0)
                            elseif walkSpeed > 16 and walkSpeed ~= 24 and walkSpeed ~= 25 then
                                color = Color3.fromRGB(255, 0, 0)
                            elseif walkSpeed == 24 then
                                color = Color3.fromRGB(255, 165, 0)
                            elseif walkSpeed == 25 then
                                color = Color3.fromRGB(172, 79, 198)
                            elseif walkSpeed == 20 then
                                color = Color3.fromRGB(255, 0, 0)
                            else
                                color = Color3.fromRGB(50, 205, 50)
                            end
                        end

                        if color then
                            if esp then
                                esp.TextLabel.TextColor3 = color
                            end
                            for _, partName in pairs({"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"}) do
                                local part = player.Character:FindFirstChild(partName)
                                local box = part and part:FindFirstChild("ESPBox")
                                if box then
                                    if isPartVisible(part) then
                                        box.Transparency = 1
                                    else
                                        box.Transparency = 0.4
                                        box.Color3 = color
                                    end
                                end
                            end
                        end
                    end
                end
            end

            espConnection = RunService.Heartbeat:Connect(updateESP)
        else
            if activatedLabel then
                activatedLabel:Destroy()
            end
            warningLabel = createStyledTextLabel("ESP has been deactivated.", Color3.fromRGB(255, 0, 0))

            if espConnection then
                espConnection:Disconnect()
            end

            for _, player in pairs(game:GetService("Players"):GetPlayers()) do
                if player.Character then
                    local head = player.Character:FindFirstChild("Head")
                    if head then
                        local esp = head:FindFirstChild("ESP")
                        if esp then
                            esp:Destroy()
                        end
                    end
                    for _, partName in pairs({"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"}) do
                        local part = player.Character:FindFirstChild(partName)
                        if part then
                            local box = part:FindFirstChild("ESPBox")
                            if box then
                                box:Destroy()
                            end
                        end
                    end
                end
            end
        end
    end
})

local seslreek = SurviTab:CreateButton({
    Name = "Spectate Slasher👁️🔪",
    Callback = function()
        local Players = game:GetService("Players")
        local player = Players.LocalPlayer
        local camera = workspace.CurrentCamera
        local tweenService = game:GetService("TweenService")

        -- Function to create and display a styled text label
        local function createStyledTextLabel(text, color)
            local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
            local textLabel = Instance.new("TextLabel", screenGui)
            textLabel.Size = UDim2.new(0, 600, 0, 100)
            textLabel.Position = UDim2.new(0.5, -300, -0.1, 0)  -- Start above the screen
            textLabel.Text = text
            textLabel.TextScaled = true
            textLabel.BackgroundTransparency = 1
            textLabel.TextColor3 = color
            textLabel.Font = Enum.Font.GothamBlack
            textLabel.TextStrokeTransparency = 0
            textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

            -- Slide-in effect
            local targetPosition = UDim2.new(0.5, -300, 0.5, -50) -- Centered position
            local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
            local slideInTween = tweenService:Create(textLabel, tweenInfo, {Position = targetPosition})
            slideInTween:Play()

            -- Smooth fade-out after a delay
            slideInTween.Completed:Connect(function()
                task.delay(4, function()
                    local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                    fadeOutTween:Play()
                    fadeOutTween.Completed:Connect(function()
                        textLabel:Destroy()
                    end)
                end)
            end)

            return textLabel
        end

        -- Function to check if there are players with specified walk speeds
        local function hasPlayersWithWalkSpeed(speeds)
            for _, p in ipairs(Players:GetPlayers()) do
                if p.Character and p.Character:FindFirstChild("Humanoid") then
                    local humanoid = p.Character:FindFirstChild("Humanoid")
                    if table.find(speeds, humanoid.WalkSpeed) then
                        return true
                    end
                end
            end
            return false
        end

        -- Function to move the camera to a player with specified walk speeds
        local function moveToPlayerWithWalkSpeed(speeds)
            for _, p in ipairs(Players:GetPlayers()) do
                if p.Character and p.Character:FindFirstChild("Humanoid") then
                    local humanoid = p.Character:FindFirstChild("Humanoid")
                    if table.find(speeds, humanoid.WalkSpeed) then
                        camera.CameraSubject = p.Character
                        camera.CameraType = Enum.CameraType.Attach
                        return
                    end
                end
            end
        end

        -- Function to create a GUI button with a background
        local function createButton()
            local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
            local button = Instance.new("TextButton", screenGui)
            local uiCorner = Instance.new("UICorner", button)
            
            button.Size = UDim2.new(0, 250, 0, 75) -- Increased size
            button.Position = UDim2.new(0.5, -125, 0.8, 0) -- Adjusted position for new size
            button.Text = "EXIT"
            button.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Red background
            button.TextColor3 = Color3.fromRGB(255, 255, 255) -- White text
            button.Font = Enum.Font.GothamBlack
            button.TextScaled = true
            button.TextStrokeTransparency = 0
            button.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

            -- Add rounded corners with a radius of 0.15
            uiCorner.CornerRadius = UDim.new(0, 15)

            -- Function to move the camera back to normal view
            button.MouseButton1Click:Connect(function()
                camera.CameraSubject = player.Character
                camera.CameraType = Enum.CameraType.Custom
                screenGui:Destroy()
            end)
        end

        -- Check for players with the specified walk speeds before proceeding
        if hasPlayersWithWalkSpeed({20, 22.5}) then
            -- Call the function to move the camera
            moveToPlayerWithWalkSpeed({20, 22.5})
            
            -- Call the function to create the button
            createButton()
        else
            -- Show warning message if no player with specified walk speed is found
            createStyledTextLabel("Warning: Please wait until Slasher is selected or spawns to use this.", Color3.fromRGB(255, 174, 66))
        end
    end,
})

SurviTab:CreateButton({
    Name = "Revive Me❤️ (LITTLE RISKY)",
    Callback = function()
        local tweenService = game:GetService("TweenService")
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
        local backpack = player:WaitForChild("Backpack")
        local originalPosition = humanoidRootPart.Position
        local targetPlayer = nil
        local Players = game:GetService("Players")
        local teleporting = true

        -- Create a styled text label
        local function createStyledTextLabel(text, color)
            local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
            local textLabel = Instance.new("TextLabel", screenGui)
            textLabel.Size = UDim2.new(0, 600, 0, 100)
            textLabel.Position = UDim2.new(0.5, -300, -0.1, 0)  -- Start above the screen
            textLabel.Text = text
            textLabel.TextScaled = true
            textLabel.BackgroundTransparency = 1
            textLabel.TextColor3 = color
            textLabel.Font = Enum.Font.GothamBlack
            textLabel.TextStrokeTransparency = 0
            textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

            -- Slide-in effect
            local targetPosition = UDim2.new(0.5, -300, 0.5, -50) -- Centered position
            local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
            local slideInTween = tweenService:Create(textLabel, tweenInfo, {Position = targetPosition})
            slideInTween:Play()

            -- Smooth fade-out after a delay
            slideInTween.Completed:Connect(function()
                task.delay(4, function()
                    local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                    fadeOutTween:Play()
                    fadeOutTween.Completed:Connect(function()
                        textLabel:Destroy()
                    end)
                end)
            end)

            return textLabel
        end

        -- Check for DisplayGun and Speed Shake
        local function checkConditions()
            if character:FindFirstChild("DisplayGun") then
                createStyledTextLabel("Warning: You can't use this in the lobby.", Color3.fromRGB(255, 174, 66))
                return false
            elseif (humanoid.WalkSpeed == 20 or humanoid.WalkSpeed == 22.5) and 
                (not backpack:FindFirstChild("Speed Shake") and not character:FindFirstChild("Speed Shake")) then
                createStyledTextLabel("Warning: You can't use this while you are slasher.", Color3.fromRGB(255, 174, 66))
                return false
            elseif humanoid.WalkSpeed == 16 then
                createStyledTextLabel("Warning: You must be injured by the slasher to use this.", Color3.fromRGB(255, 174, 66))
                return false
            end
            return true
        end

        -- Function to find a target player with WalkSpeed of 20 or 22.5
        local function findTargetPlayer()
            for _, otherPlayer in pairs(Players:GetPlayers()) do
                if otherPlayer ~= player and otherPlayer.Character then
                    local otherHumanoid = otherPlayer.Character:FindFirstChildOfClass("Humanoid")
                    if otherHumanoid and (otherHumanoid.WalkSpeed == 20 or otherHumanoid.WalkSpeed == 22.5) then
                        return otherPlayer
                    end
                end
            end
            return nil
        end

        -- Function to teleport to the target player
        local function teleportToTarget()
            if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                humanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            end
        end

        -- Function to spam teleport and handle ForceField check
        local function spamTeleport()
            targetPlayer = findTargetPlayer()

            -- Spam teleport to player with WalkSpeed 20 or 22.5
            while teleporting do
                if targetPlayer then
                    teleportToTarget()
                else
                    targetPlayer = findTargetPlayer()
                end
                
                -- Check for ForceField
                if character:FindFirstChild("ForceField") then
                    teleporting = false
                    humanoidRootPart.CFrame = CFrame.new(originalPosition) -- Teleport back to original position
                    break
                end
                wait(0.05)
            end
        end

        -- Check the conditions before teleporting
        if checkConditions() then
            createStyledTextLabel("Healing...", Color3.fromRGB(0, 255, 0)) -- Show message before starting
            spamTeleport()
        end
    end
})

local toggle = false
local connection
local PseudoAnchor
local CanInvis = true
local IsInvisible = false
local FakeCharacter
local Part
local Player = game.Players.LocalPlayer
local RealCharacter = Player.Character or Player.CharacterAdded:Wait()
local Transparency = true
local NoClip = false
local LightPart -- Part for light to indicate player's position
local FaceDecal
local tweenService = game:GetService("TweenService")
local originalTransparency = {}

SurviTab:CreateToggle({
    Name = "Invisible To Slasher🔪👁️🚫 (Disables Game Interaction)",
    CurrentValue = false,
    Flag = "SlasherToggle",
    Callback = function(Value)
    -- Function to create styled text labels
local function createStyledTextLabel(text, color)
    local screenGui = Instance.new("ScreenGui", Player:WaitForChild("PlayerGui"))
    local textLabel = Instance.new("TextLabel", screenGui)
    textLabel.Size = UDim2.new(0, 600, 0, 100)
    textLabel.Position = UDim2.new(0.5, -300, 0.1, 0)
    textLabel.Text = text
    textLabel.TextScaled = true
    textLabel.BackgroundTransparency = 1
    textLabel.TextColor3 = color
    textLabel.Font = Enum.Font.GothamBlack
    textLabel.TextStrokeTransparency = 0
    textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)

    local targetPosition = UDim2.new(0.5, -300, 0.5, -50)
    local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local slideInTween = tweenService:Create(textLabel, tweenInfo, {Position = targetPosition})
    slideInTween:Play()

    slideInTween.Completed:Connect(function()
        task.delay(4, function()
            local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
            fadeOutTween:Play()
            fadeOutTween.Completed:Connect(function()
                textLabel:Destroy()
            end)
        end)
    end)
end

-- Function to store the original transparency of the character parts
local function storeOriginalTransparency()
    for _, part in pairs(RealCharacter:GetDescendants()) do
        if part:IsA("BasePart") and not originalTransparency[part] then
            originalTransparency[part] = part.Transparency
        end
    end
end

-- Function to restore the original transparency of the character parts
local function restoreOriginalTransparency()
    for part, transparency in pairs(originalTransparency) do
        if part and part:IsA("BasePart") then
            part.Transparency = transparency
        end
    end
end

        local character = Player.Character or Player.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")

        -- Condition checks for DisplayGun and WalkSpeed
        if character:FindFirstChild("DisplayGun") then
            createStyledTextLabel("Warning: You can't become invisible in the lobby, wait until game starts.", Color3.fromRGB(255, 174, 66))
            return
        elseif humanoid.WalkSpeed == 20 or humanoid.WalkSpeed == 22.5 then
            createStyledTextLabel("Warning: You can't become invisible while being slasher, it will glitch.", Color3.fromRGB(255, 174, 66))
            return
        end

        toggle = Value

        -- Function to initialize the fake character
        local function setupFakeCharacter()
            RealCharacter.Archivable = true
            FakeCharacter = RealCharacter:Clone()

            -- Create ground part to hold the fake character
            Part = Instance.new("Part", workspace)
            Part.Anchored = true
            Part.Size = Vector3.new(200, 1, 200)
            Part.CFrame = CFrame.new(0, -500, 0)
            Part.CanCollide = true

            -- Create a light part to indicate player's position
            LightPart = Instance.new("Part", workspace)
            LightPart.Anchored = true
            LightPart.CanCollide = false
            LightPart.Shape = Enum.PartType.Ball
            LightPart.Size = Vector3.new(5, 5, 5)
            LightPart.CFrame = RealCharacter.HumanoidRootPart.CFrame
            local light = Instance.new("PointLight", LightPart)
            light.Brightness = 2
            light.Range = 10

            -- Set fake character's position and transparency
            FakeCharacter.Parent = workspace
            FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

            if Transparency then
                for _, v in pairs(FakeCharacter:GetDescendants()) do
                    if v:IsA("BasePart") then
                        v.Transparency = 1
                    elseif v:IsA("Decal") and v.Name == "face" then
                        v.Transparency = 1 -- Hide face decal
                    end
                end
            end

            for _, v in pairs(RealCharacter:GetChildren()) do
                if v:IsA("LocalScript") then
                    local clone = v:Clone()
                    clone.Disabled = true
                    clone.Parent = FakeCharacter
                end
            end

            PseudoAnchor = FakeCharacter.HumanoidRootPart
        end

        -- Function to handle invisibility toggle
        local function Invisible(enable)
            if enable and not IsInvisible then
                storeOriginalTransparency() -- Store transparency
                local StoredCF = RealCharacter.HumanoidRootPart.CFrame
                RealCharacter.HumanoidRootPart.CFrame = FakeCharacter.HumanoidRootPart.CFrame
                FakeCharacter.HumanoidRootPart.CFrame = StoredCF
                RealCharacter.Humanoid:UnequipTools()
                Player.Character = FakeCharacter
                workspace.CurrentCamera.CameraSubject = FakeCharacter.Humanoid
                PseudoAnchor = RealCharacter.HumanoidRootPart

                for _, v in pairs(FakeCharacter:GetChildren()) do
                    if v:IsA("LocalScript") then
                        v.Disabled = false
                    end
                end

                -- Hide real character's face decal
                for _, v in pairs(RealCharacter:GetChildren()) do
                    if v:IsA("Decal") and v.Name == "face" then
                        FaceDecal = v
                        v.Transparency = 1
                    end
                end

                IsInvisible = true
            elseif not enable and IsInvisible then
                local StoredCF = FakeCharacter.HumanoidRootPart.CFrame
                FakeCharacter.HumanoidRootPart.CFrame = RealCharacter.HumanoidRootPart.CFrame
                RealCharacter.HumanoidRootPart.CFrame = StoredCF

                FakeCharacter.Humanoid:UnequipTools()
                Player.Character = RealCharacter
                workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid
                PseudoAnchor = FakeCharacter.HumanoidRootPart

                for _, v in pairs(FakeCharacter:GetChildren()) do
                    if v:IsA("LocalScript") then
                        v.Disabled = true
                    end
                end

                -- Restore real character's face decal
                if FaceDecal then
                    FaceDecal.Transparency = 0
                end

                restoreOriginalTransparency() -- Restore transparency
                IsInvisible = false
            end
        end

        if toggle then
            if not FakeCharacter then
                setupFakeCharacter()
            end
            createStyledTextLabel("Invisibility enabled!", Color3.fromRGB(0, 255, 0)) -- Message when invisibility is ON
            Invisible(true) -- Enable invisibility when toggled on

            -- NoClip and idle state for fake character
            connection = game:GetService("RunService").RenderStepped:Connect(function()
                if NoClip and FakeCharacter then
                    FakeCharacter.Humanoid:ChangeState(11) -- NoClip mode
                end
                if PseudoAnchor then
                    PseudoAnchor.CFrame = Part.CFrame * CFrame.new(0, 5, 0) -- Keep the fake character in place
                end

                -- Keep the light part above the real character
                if LightPart and RealCharacter and RealCharacter:FindFirstChild("HumanoidRootPart") then
                    LightPart.CFrame = RealCharacter.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
                end
            end)
        else
            if IsInvisible then
                Invisible(false) -- Disable invisibility when toggled off
            end
            if FakeCharacter then
                FakeCharacter:Destroy()
                FakeCharacter = nil
            end
            if Part then
                Part:Destroy()
                Part = nil
            end
            if LightPart then
                LightPart:Destroy()
                LightPart = nil
            end
            if connection then
                connection:Disconnect()
                connection = nil
            end
            createStyledTextLabel("Invisibility disabled.", Color3.fromRGB(255, 0, 0)) -- Message when invisibility is OFF
        end
    end
})

ExtrTab:CreateButton({
    Name = "Teleport To Lobby🏠",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character
        local lobbySpawns = game.Workspace:FindFirstChild("Lobby"):FindFirstChild("LobbySpawns")

        if lobbySpawns and character then
            local spawnPoints = lobbySpawns:GetChildren()
            local randomSpawn = spawnPoints[math.random(1, #spawnPoints)]
            if randomSpawn:IsA("BasePart") then
                character:MoveTo(randomSpawn.Position)
            end
        end
    end
})

ExtrTab:CreateButton({
    Name = "Teleport To Current Map🌆",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character
        local survivorSpawns = game.Workspace:FindFirstChild("CurrentMap"):FindFirstChild("SurvivorSpawns")

        if survivorSpawns and character then
            local spawnPoints = survivorSpawns:GetChildren()
            local randomSpawn = spawnPoints[math.random(1, #spawnPoints)]
            if randomSpawn:IsA("BasePart") then
                character:MoveTo(randomSpawn.Position)
            end
        end
    end
})

-- Collect All Coins Button
ExtrTab:CreateButton({
    Name = "Collect All Coins💰",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character
        local hrp = character:WaitForChild("HumanoidRootPart")
        local originalPosition = hrp.CFrame

        local function collectCoins()
            local coinsModel = workspace:FindFirstChild("CurrentMap"):FindFirstChild("Coins")
            if coinsModel then
                local coins = coinsModel:GetChildren()
                while #coins > 0 do
                    for _, coin in pairs(coins) do
                        if coin:IsA("BasePart") and coin.Name == "Coin" then
                            hrp.CFrame = coin.CFrame
                            wait(0.05) -- Adjust delay to control teleport speed
                            coin:Destroy()
                        end
                    end
                    coins = coinsModel:GetChildren()
                end
            end

            hrp.CFrame = originalPosition
        end

        collectCoins()
    end
})


ExtrTab:CreateButton({
    Name = "Teleport To Slasher🔪",
    Callback = function()
        -- Find the player with 20 walk speed
        local targetPlayer = nil
        for _, player in ipairs(game.Players:GetPlayers()) do
            if player.Character and player.Character:FindFirstChild("Humanoid") then
                local humanoid = player.Character.Humanoid
                if humanoid.WalkSpeed == 20 then
                    targetPlayer = player
                    break
                end
            end
        end

        -- Teleport the player to the target player
        if targetPlayer then
            local localPlayer = game.Players.LocalPlayer
            if localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart") then
                localPlayer.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            end
        else
            warn("Slasher has not been picked yet!.")
        end
    end
})

ExtrTab:CreateButton({
    Name = "Teleport To Random Player🧑‍🤝‍🧑",
    Callback = function()
        local players = game:GetService("Players")
        local localPlayer = players.LocalPlayer

        -- Function to teleport to a player
        local function teleportToPlayer(player)
            if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                localPlayer.Character:SetPrimaryPartCFrame(player.Character.HumanoidRootPart.CFrame)
            end
        end
   
        -- Get a list of players that don't have DisplayGun
        local validPlayers = {}
        for _, player in pairs(players:GetPlayers()) do
            if player ~= localPlayer and player.Character and not player.Character:FindFirstChild("DisplayGun") then
                table.insert(validPlayers, player)
            end
        end

        -- Teleport to a random valid player
        if #validPlayers > 0 then
            local randomPlayer = validPlayers[math.random(1, #validPlayers)]
            teleportToPlayer(randomPlayer)
        else
            print("No valid players found.")
        end
    end
})

ExtrTab:CreateButton({
    Name = "Teleport To Random Player (Lobby)🧑‍🤝‍🧑",
    Callback = function()
        local players = game:GetService("Players")
        local localPlayer = players.LocalPlayer

        -- Function to teleport to a player
        local function teleportToPlayer(player)
            if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                localPlayer.Character:SetPrimaryPartCFrame(player.Character.HumanoidRootPart.CFrame)
            end
        end

        -- Get a list of players that have DisplayGun
        local validPlayers = {}
        for _, player in pairs(players:GetPlayers()) do
            if player ~= localPlayer and player.Character and player.Character:FindFirstChild("DisplayGun") then
                table.insert(validPlayers, player)
            end
        end

        -- Teleport to a random valid player
        if #validPlayers > 0 then
            local randomPlayer = validPlayers[math.random(1, #validPlayers)]
            teleportToPlayer(randomPlayer)
        else
            print("No players with DisplayGun found.")
        end
    end
})

ExtrTab:CreateButton({
    Name = "Remove Barriers (Map)🚫🧱",
    Callback = function()
        local workspace = game:GetService("Workspace")
        local currentMap = workspace:FindFirstChild("CurrentMap")

        if currentMap then
            local otherFolder = currentMap:FindFirstChild("Other")
            local extraModel = currentMap:FindFirstChild("Extra")
            local invisWalls = nil

            if otherFolder then
                invisWalls = otherFolder:FindFirstChild("InvisWalls")
            end

            if not invisWalls and extraModel and extraModel:IsA("Model") then
                invisWalls = extraModel:FindFirstChild("InvisWalls")
            end

            if invisWalls and invisWalls:IsA("Model") then
                invisWalls:Destroy()
            end
        end
    end
})

ExtrTab:CreateButton({
    Name = "Remove Barriers (Lobby)🚫🧱",
    Callback = function()
        local workspace = game:GetService("Workspace")
        local lobbyFolder = workspace:FindFirstChild("Lobby")

        if lobbyFolder then
            local invisModel = lobbyFolder:FindFirstChild("Invis")

            if invisModel and invisModel:IsA("Model") then
                invisModel:Destroy()
            end
        end
    end
})

CrdTab:CreateButton({
    Name = "👑Creator: Jor🛠️.",
    Callback = function()
    end
})

CrdTab:CreateButton({
    Name = "📜Scripter: WaveAI🌊 & Jor🛠️.",
    Callback = function()
    end
})

CrdTab:CreateButton({
    Name = "🧪Tester: Jor🛠️.",
    Callback = function()
    end
})

CrdTab:CreateButton({
    Name = "💻GUI Creator/Contributor: shlexware💻.",
    Callback = function()
    end
})

CrdTab:CreateButton({
    Name = "💻GUI Type: Rayfield💻.",
    Callback = function()
    end
})

SlrTab:CreateButton({
    Name = "Comming Soon..",
    Callback = function()
    end
})
