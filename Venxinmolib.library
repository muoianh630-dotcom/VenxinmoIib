local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local UserInputService = game:GetService("UserInputService")

local Library = {}

function Library:CreateWindow(titleText)
    if CoreGui:FindFirstChild("CustomDynamicMenu") then
        CoreGui.CustomDynamicMenu:Destroy()
    end

    local ScreenGui = Instance.new("ScreenGui", CoreGui)
    ScreenGui.Name = "CustomDynamicMenu"
    ScreenGui.IgnoreGuiInset = true

    local function MakeDraggable(topbar, object)
        local dragging, dragInput, dragStart, startPos
        topbar.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                dragging, dragStart, startPos = true, input.Position, object.Position
                input.Changed:Connect(function() if input.UserInputState == Enum.UserInputState.End then dragging = false end end)
            end
        end)
        topbar.InputChanged:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then dragInput = input end
        end)
        UserInputService.InputChanged:Connect(function(input)
            if input == dragInput and dragging then
                local delta = input.Position - dragStart
                object.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
            end
        end)
    end

    local MainFrame = Instance.new("Frame", ScreenGui)
    MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
    MainFrame.Position = UDim2.new(0.5, -250, 0.5, -175)
    MainFrame.Size = UDim2.new(0, 500, 0, 350)
    Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 8)
    Instance.new("UIStroke", MainFrame).Color = Color3.fromRGB(50, 50, 50)

    local TopBar = Instance.new("Frame", MainFrame)
    TopBar.Size = UDim2.new(1, 0, 0, 38)
    TopBar.BackgroundColor3 = Color3.fromRGB(24, 24, 24)
    TopBar.BorderSizePixel = 0
    TopBar.ZIndex = 2
    Instance.new("UICorner", TopBar).CornerRadius = UDim.new(0, 8)
    MakeDraggable(TopBar, MainFrame)

    local CloseMenuButton = Instance.new("TextButton", TopBar)
    CloseMenuButton.Size = UDim2.new(0, 28, 0, 28)
    CloseMenuButton.Position = UDim2.new(1, -36, 0, 5)
    CloseMenuButton.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    CloseMenuButton.Text = "X"
    CloseMenuButton.TextColor3 = Color3.fromRGB(200, 200, 200)
    CloseMenuButton.ZIndex = 2
    Instance.new("UICorner", CloseMenuButton).CornerRadius = UDim.new(0, 5)

    local ToggleMenuButton = Instance.new("TextButton", TopBar)
    ToggleMenuButton.Size = UDim2.new(0, 28, 0, 28)
    ToggleMenuButton.Position = UDim2.new(1, -68, 0, 5)
    ToggleMenuButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    ToggleMenuButton.Text = "-"
    ToggleMenuButton.TextColor3 = Color3.fromRGB(200, 200, 200)
    ToggleMenuButton.ZIndex = 2
    Instance.new("UICorner", ToggleMenuButton).CornerRadius = UDim.new(0, 5)

    local TitleLabel = Instance.new("TextLabel", TopBar)
    TitleLabel.Size = UDim2.new(1, -80, 1, 0)
    TitleLabel.Position = UDim2.new(0, 12, 0, 0)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = titleText or "Dynamic Menu"
    TitleLabel.TextColor3 = Color3.fromRGB(210, 210, 210)
    TitleLabel.TextSize = 13
    TitleLabel.Font = Enum.Font.GothamBold
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
    TitleLabel.ZIndex = 2

    -- Tính toán chiều rộng tối ưu khi thu nhỏ dựa vào độ dài chữ tiêu đề
    local textService = game:GetService("TextService")
    local textSize = textService:GetTextSize(TitleLabel.Text, TitleLabel.TextSize, TitleLabel.Font, Vector2.new(1000, 38))
    local minimizedWidth = math.clamp(textSize.X + 90, 200, 500) -- Tự dãn rộng theo chữ, nhỏ nhất là 200

    local ContainerHolder = Instance.new("Frame", MainFrame)
    ContainerHolder.Size = UDim2.new(1, 0, 1, -38)
    ContainerHolder.Position = UDim2.new(0, 0, 0, 38)
    ContainerHolder.BackgroundTransparency = 1
    ContainerHolder.ClipsDescendants = true

    CloseMenuButton.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

    local menuVisible = true
    ToggleMenuButton.MouseButton1Click:Connect(function()
        menuVisible = not menuVisible
        if menuVisible then
            ContainerHolder.Visible = true
            TweenService:Create(MainFrame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(0, 500, 0, 350)}):Play()
        else
            -- Co lại vừa khít với độ dài của tên tiêu đề, không sợ bị đè chữ nữa
            TweenService:Create(MainFrame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(0, minimizedWidth, 0, 38)}):Play()
            task.wait(0.25)
            if not menuVisible then
                ContainerHolder.Visible = false
            end
        end
        ToggleMenuButton.Text = menuVisible and "-" or "+"
    end)

    local TabBar = Instance.new("ScrollingFrame", ContainerHolder)
    TabBar.Size = UDim2.new(0, 130, 1, -10)
    TabBar.Position = UDim2.new(0, 8, 0, 6)
    TabBar.BackgroundTransparency = 1
    TabBar.ScrollBarThickness = 2
    local TabListLayout = Instance.new("UIListLayout", TabBar)
    TabListLayout.Padding = UDim.new(0, 6)
    TabListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        TabBar.CanvasSize = UDim2.new(0, 0, 0, TabListLayout.AbsoluteContentSize.Y + 10)
    end)

    local ContentContainer = Instance.new("Frame", ContainerHolder)
    ContentContainer.Size = UDim2.new(1, -148, 1, -10)
    ContentContainer.Position = UDim2.new(0, 142, 0, 6)
    ContentContainer.BackgroundTransparency = 1

    local Tabs = {}
    local FirstTab = true

    local WindowObj = {}

    function WindowObj:CreateTab(tabName, iconId)
        local scrollingFrame = Instance.new("ScrollingFrame", ContentContainer)
        scrollingFrame.Size = UDim2.new(1, 0, 1, 0)
        scrollingFrame.BackgroundTransparency = 1
        scrollingFrame.ScrollBarThickness = 3
        scrollingFrame.Visible = FirstTab
        
        local UIListLayout = Instance.new("UIListLayout", scrollingFrame)
        UIListLayout.Padding = UDim.new(0, 6)
        UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
            scrollingFrame.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 10)
        end)

        local btn = Instance.new("TextButton", TabBar)
        btn.Size = UDim2.new(1, 0, 0, 34)
        btn.BackgroundColor3 = FirstTab and Color3.fromRGB(45, 45, 45) or Color3.fromRGB(26, 26, 26)
        btn.BackgroundTransparency = FirstTab and 0 or 0.5
        btn.Text = "       " .. tabName
        btn.TextColor3 = FirstTab and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(170, 170, 170)
        btn.TextSize = 12
        btn.Font = Enum.Font.GothamMedium
        btn.TextXAlignment = Enum.TextXAlignment.Left
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
        
        local icon = Instance.new("ImageLabel", btn)
        icon.Size = UDim2.new(0, 16, 0, 16)
        icon.Position = UDim2.new(0, 8, 0.5, -8)
        icon.BackgroundTransparency = 1
        icon.Image = iconId or "rbxassetid://6031094607"
        icon.ImageColor3 = FirstTab and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(170, 170, 170)
        
        btn.MouseButton1Click:Connect(function()
            for _, tabData in pairs(Tabs) do
                tabData.Content.Visible = false
                tabData.Button.BackgroundColor3 = Color3.fromRGB(26, 26, 26)
                tabData.Button.BackgroundTransparency = 0.5
                tabData.Button.TextColor3 = Color3.fromRGB(170, 170, 170)
                tabData.Button.ImageLabel.ImageColor3 = Color3.fromRGB(170, 170, 170)
            end
            scrollingFrame.Visible = true
            btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
            btn.BackgroundTransparency = 0
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            icon.ImageColor3 = Color3.fromRGB(255, 255, 255)
        end)
        
        Tabs[tabName] = {Button = btn, Content = scrollingFrame, Icon = icon}
        FirstTab = false

        local TabObj = {}

        function TabObj:AddToggle(text, callback)
            local frame = Instance.new("Frame", scrollingFrame)
            frame.Size = UDim2.new(1, 0, 0, 36)
            frame.BackgroundColor3 = Color3.fromRGB(24, 24, 24)
            Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
            
            local label = Instance.new("TextLabel", frame)
            label.Size = UDim2.new(1, -50, 1, 0)
            label.Position = UDim2.new(0, 10, 0, 0)
            label.BackgroundTransparency = 1
            label.Text = text
            label.TextColor3 = Color3.fromRGB(200, 200, 200)
            label.TextSize = 12
            label.Font = Enum.Font.Gotham
            label.TextXAlignment = Enum.TextXAlignment.Left
            
            local toggleBtn = Instance.new("TextButton", frame)
            toggleBtn.Size = UDim2.new(0, 40, 0, 22)
            toggleBtn.Position = UDim2.new(1, -48, 0.5, -11)
            toggleBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
            toggleBtn.Text = ""
            Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(1, 0)
            
            local indicator = Instance.new("Frame", toggleBtn)
            indicator.Size = UDim2.new(0, 16, 0, 16)
            indicator.Position = UDim2.new(0, 3, 0.5, -8)
            indicator.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
            Instance.new("UICorner", indicator).CornerRadius = UDim.new(1, 0)
            
            local state = false
            toggleBtn.MouseButton1Click:Connect(function()
                state = not state
                TweenService:Create(toggleBtn, TweenInfo.new(0.15), {BackgroundColor3 = state and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(45, 45, 45)}):Play()
                TweenService:Create(indicator, TweenInfo.new(0.15), {Position = state and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8), BackgroundColor3 = state and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(200, 200, 200)}):Play()
                pcall(function() callback(state) end)
            end)
        end

        function TabObj:AddButton(text, callback)
            local btnComp = Instance.new("TextButton", scrollingFrame)
            btnComp.Size = UDim2.new(1, 0, 0, 36)
            btnComp.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
            btnComp.Text = text
            btnComp.TextColor3 = Color3.fromRGB(220, 220, 220)
            btnComp.TextSize = 12
            btnComp.Font = Enum.Font.GothamBold
            Instance.new("UICorner", btnComp).CornerRadius = UDim.new(0, 6)
            
            btnComp.MouseButton1Click:Connect(function)
                pcall(function() callback() end)
            end)
        end

        function TabObj:AddTextBox(text, placeholder, callback)
            local frame = Instance.new("Frame", scrollingFrame)
            frame.Size = UDim2.new(1, 0, 0, 36)
            frame.BackgroundColor3 = Color3.fromRGB(24, 24, 24)
            Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
            
            local label = Instance.new("TextLabel", frame)
            label.Size = UDim2.new(0.5, -10, 1, 0)
            label.Position = UDim2.new(0, 10, 0, 0)
            label.BackgroundTransparency = 1
            label.Text = text
            label.TextColor3 = Color3.fromRGB(200, 200, 200)
            label.TextSize = 12
            label.Font = Enum.Font.Gotham
            label.TextXAlignment = Enum.TextXAlignment.Left
            
            local textBox = Instance.new("TextBox", frame)
            textBox.Size = UDim2.new(0.5, -15, 0, 26)
            textBox.Position = UDim2.new(0.5, 0, 0.5, -13)
            textBox.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
            textBox.Text = ""
            textBox.PlaceholderText = placeholder or "Input..."
            textBox.TextColor3 = Color3.fromRGB(255, 255, 255)
            textBox.PlaceholderColor3 = Color3.fromRGB(120, 120, 120)
            textBox.TextSize = 12
            textBox.Font = Enum.Font.Gotham
            Instance.new("UICorner", textBox).CornerRadius = UDim.new(0, 4)
            
            textBox.FocusLost:Connect(function(enterPressed)
                pcall(function() callback(textBox.Text, enterPressed) end)
            end)
        end

        return TabObj
    end

    return WindowObj
end

return Library
