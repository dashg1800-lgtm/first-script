
-- DARK PRO GUI
-- Created with ChatGPT
-- Roblox LocalScript

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Remove old GUI
local oldGui = playerGui:FindFirstChild("DarkProGUI")
if oldGui then
    oldGui:Destroy()
end

-- Theme
local BG = Color3.fromRGB(15, 15, 19)
local PANEL = Color3.fromRGB(24, 24, 30)
local RED = Color3.fromRGB(220, 35, 55)
local WHITE = Color3.fromRGB(245, 245, 245)
local GREY = Color3.fromRGB(155, 155, 165)

-- Screen GUI
local gui = Instance.new("ScreenGui")
gui.Name = "DarkProGUI"
gui.ResetOnSpawn = false
gui.Parent = playerGui

-- Main window
local main = Instance.new("Frame")
main.Name = "Main"
main.Size = UDim2.fromOffset(540, 360)
main.Position = UDim2.new(0.5, -270, 0.5, -180)
main.BackgroundColor3 = BG
main.BorderSizePixel = 0
main.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = main

local stroke = Instance.new("UIStroke")
stroke.Color = RED
stroke.Thickness = 1.5
stroke.Parent = main

-- Top bar
local top = Instance.new("Frame")
top.Size = UDim2.new(1, 0, 0, 48)
top.BackgroundColor3 = PANEL
top.BorderSizePixel = 0
top.Parent = main

local topCorner = Instance.new("UICorner")
topCorner.CornerRadius = UDim.new(0, 12)
topCorner.Parent = top

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -60, 1, 0)
title.Position = UDim2.fromOffset(16, 0)
title.BackgroundTransparency = 1
title.Text = "DARK PRO  /  V1.0"
title.TextColor3 = RED
title.TextSize = 18
title.Font = Enum.Font.GothamBold
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = top

-- Close button
local close = Instance.new("TextButton")
close.Size = UDim2.fromOffset(36, 32)
close.Position = UDim2.new(1, -44, 0, 8)
close.BackgroundColor3 = RED
close.Text = "X"
close.TextColor3 = WHITE
close.TextSize = 16
close.Font = Enum.Font.GothamBold
close.Parent = top

Instance.new("UICorner", close).CornerRadius = UDim.new(0, 7)

-- Sidebar
local sidebar = Instance.new("Frame")
sidebar.Size = UDim2.new(0, 145, 1, -58)
sidebar.Position = UDim2.fromOffset(10, 52)
sidebar.BackgroundColor3 = PANEL
sidebar.BorderSizePixel = 0
sidebar.Parent = main

Instance.new("UICorner", sidebar).CornerRadius = UDim.new(0, 9)

local sideLayout = Instance.new("UIListLayout")
sideLayout.Padding = UDim.new(0, 9)
sideLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
sideLayout.SortOrder = Enum.SortOrder.LayoutOrder
sideLayout.Parent = sidebar

local sidePadding = Instance.new("UIPadding")
sidePadding.PaddingTop = UDim.new(0, 12)
sidePadding.Parent = sidebar

-- Content area
local content = Instance.new("Frame")
content.Size = UDim2.new(1, -175, 1, -68)
content.Position = UDim2.fromOffset(165, 58)
content.BackgroundTransparency = 1
content.Parent = main

local pages = {}
local tabButtons = {}
local currentTab = nil

local function createPage(name)
    local page = Instance.new("ScrollingFrame")
    page.Name = name
    page.Size = UDim2.fromScale(1, 1)
    page.BackgroundTransparency = 1
    page.BorderSizePixel = 0
    page.ScrollBarThickness = 3
    page.ScrollBarImageColor3 = RED
    page.Visible = false
    page.CanvasSize = UDim2.new(0, 0, 0, 0)
    page.AutomaticCanvasSize = Enum.AutomaticSize.Y
    page.Parent = content

    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 10)
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Parent = page

    local padding = Instance.new("UIPadding")
    padding.PaddingTop = UDim.new(0, 5)
    padding.PaddingBottom = UDim.new(0, 10)
    padding.PaddingLeft = UDim.new(0, 5)
    padding.PaddingRight = UDim.new(0, 5)
    padding.Parent = page

    pages[name] = page
    return page
end

local function createLabel(parent, text)
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -10, 0, 36)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = WHITE
    label.TextSize = 15
    label.Font = Enum.Font.GothamMedium
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = parent
    return label
end

local function createButton(parent, text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -10, 0, 40)
    button.BackgroundColor3 = PANEL
    button.Text = text
    button.TextColor3 = WHITE
    button.TextSize = 14
    button.Font = Enum.Font.GothamBold
    button.AutoButtonColor = false
    button.Parent = parent

    Instance.new("UICorner", button).CornerRadius =
        UDim.new(0, 7)

    local outline = Instance.new("UIStroke")
    outline.Color = Color3.fromRGB(55, 55, 65)
    outline.Parent = button

    button.MouseEnter:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.15), {
            BackgroundColor3 = RED
        }):Play()
    end)

    button.MouseLeave:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.15), {
            BackgroundColor3 = PANEL
        }):Play()
    end)

    button.Activated:Connect(callback)
    return button
end

local function createTab(name)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -16, 0, 38)
    button.BackgroundColor3 = BG
    button.Text = "  " .. name
    button.TextColor3 = GREY
    button.TextSize = 14
    button.Font = Enum.Font.GothamBold
    button.TextXAlignment = Enum.TextXAlignment.Left
    button.Parent = sidebar

    Instance.new("UICorner", button).CornerRadius =
        UDim.new(0, 7)

    local page = createPage(name)

    button.Activated:Connect(function()
        for tabName, tabPage in pairs(pages) do
            tabPage.Visible = tabName == name
        end

        for tabName, tabButton in pairs(tabButtons) do
            tabButton.BackgroundColor3 =
                tabName == name and RED or BG

            tabButton.TextColor3 =
                tabName == name and WHITE or GREY
        end

        currentTab = name
    end)

    tabButtons[name] = button
    return page
end

-- Create tabs
local home = createTab("HOME")
local playerPage = createTab("PLAYER")
local settings = createTab("SETTINGS")

-- HOME PAGE
createLabel(home, "WELCOME TO DARK PRO")
createLabel(home, "Welcome, " .. player.DisplayName)
createLabel(home, "Your personal Roblox control panel.")

createButton(home, "PRINT HELLO", function()
    print("Hello from Dark Pro GUI!")
end)

createButton(home, "SHOW NOTIFICATION", function()
    game:GetService("StarterGui"):SetCore(
        "SendNotification",
        {
            Title = "DARK PRO",
            Text = "GUI is working!",
            Duration = 3
        }
    )
end)

-- PLAYER PAGE
createLabel(playerPage, "PLAYER INFORMATION")

createLabel(playerPage, "Display Name: " .. player.DisplayName)
createLabel(playerPage, "Username: @" .. player.Name)
createLabel(playerPage, "User ID: " .. player.UserId)

local speedLabel = createLabel(playerPage, "WalkSpeed: Loading...")

local function updatePlayerInfo()
    local character = player.Character
    local humanoid = character and
        character:FindFirstChildOfClass("Humanoid")

    if humanoid then
        speedLabel.Text = "WalkSpeed: " .. humanoid.WalkSpeed
    else
        speedLabel.Text = "WalkSpeed: Not available"
    end
end

createButton(playerPage, "REFRESH PLAYER INFO", updatePlayerInfo)

-- SETTINGS PAGE
createLabel(settings, "GUI SETTINGS")

local accentRed = true

createButton(settings, "CHANGE ACCENT COLOR", function()
    accentRed = not accentRed

    local newColor = accentRed
        and Color3.fromRGB(220, 35, 55)
        or Color3.fromRGB(135, 65, 255)

    stroke.Color = newColor

    for _, button in pairs(tabButtons) do
        if button.Text:find(currentTab or "") then
            button.BackgroundColor3 = newColor
        end
    end

    title.TextColor3 = newColor
end)

createButton(settings, "RESET GUI POSITION", function()
    main.Position = UDim2.new(0.5, -270, 0.5, -180)
end)

-- Default tab
tabButtons["HOME"].BackgroundColor3 = RED
tabButtons["HOME"].TextColor3 = WHITE
pages["HOME"].Visible = true
currentTab = "HOME"

-- Draggable window
local dragging = false
local dragStart
local startPosition

top.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then

        dragging = true
        dragStart = input.Position
        startPosition = main.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (
        input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch
    ) then

        local delta = input.Position - dragStart

        main.Position = UDim2.new(
            startPosition.X.Scale,
            startPosition.X.Offset + delta.X,
            startPosition.Y.Scale,
            startPosition.Y.Offset + delta.Y
        )
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end)

-- Show / Hide
local function toggleGUI()
    main.Visible = not main.Visible
end

close.Activated:Connect(toggleGUI)

UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end

    if input.KeyCode == Enum.KeyCode.RightShift then
        toggleGUI()
    end
end)

-- Opening animation
main.Size = UDim2.fromOffset(0, 0)

TweenService:Create(
    main,
    TweenInfo.new(0.45, Enum.EasingStyle.Back, Enum.EasingDirection.Out),
    {Size = UDim2.fromOffset(540, 360)}
):Play()

print("DARK PRO GUI loaded successfully!")
