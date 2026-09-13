--[[
  Developer: Shahd - shhode320
  Version: 108 Colors + Spam + Resizing + Draggable + Fonts
]]

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local StarterGui = game:GetService("StarterGui")

local localPlayer = Players.LocalPlayer
local pImageId = "rbxassetid://134763193488874"
local spamming = false

-- إنشاء الواجهة
local sg = Instance.new("ScreenGui")
sg.Name = "Shahd_Mega_Pro"
sg.ResetOnSpawn = false
sg.Parent = localPlayer.PlayerGui

-- دالة السحب
local function makeDraggable(frame)
    local dragging, dragInput, dragStart, startPos
    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true; dragStart = input.Position; startPos = frame.Position
            input.Changed:Connect(function() if input.UserInputState == Enum.UserInputState.End then dragging = false end end)
        end
    end)
    frame.InputChanged:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then dragInput = input end end)
    UserInputService.InputChanged:Connect(function(input) if input == dragInput and dragging then local delta = input.Position - dragStart; frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y) end end)
end

-- اللوحة الرئيسية
local mainFrame = Instance.new("Frame", sg)
mainFrame.Size = UDim2.new(0, 450, 0, 420)
mainFrame.Position = UDim2.new(0.5, -225, 0.5, -210)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
mainFrame.Active = true
mainFrame.ClipsDescendants = true
makeDraggable(mainFrame)

Instance.new("UICorner", mainFrame)
local mainStroke = Instance.new("UIStroke", mainFrame)
mainStroke.Color = Color3.fromRGB(255, 105, 180); mainStroke.Thickness = 3

-- ميزة تغيير حجم اللائحة
local resizeHandle = Instance.new("TextButton", mainFrame)
resizeHandle.Size = UDim2.new(0, 20, 0, 20); resizeHandle.Position = UDim2.new(1, -20, 1, -20)
resizeHandle.Text = "◢"; resizeHandle.TextColor3 = Color3.fromRGB(255, 105, 180); resizeHandle.BackgroundTransparency = 1; resizeHandle.TextScaled = true

local resizing = false
resizeHandle.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then resizing = true end end)
UserInputService.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then resizing = false end end)
UserInputService.InputChanged:Connect(function(input)
    if resizing and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local mousePos = UserInputService:GetMouseLocation()
        local newSizeX = mousePos.X - mainFrame.AbsolutePosition.X
        local newSizeY = (mousePos.Y - 36) - mainFrame.AbsolutePosition.Y
        mainFrame.Size = UDim2.new(0, math.max(300, newSizeX), 0, math.max(200, newSizeY))
    end
end)

-- الهيدر
local header = Instance.new("Frame", mainFrame)
header.Size = UDim2.new(1, 0, 0, 60); header.BackgroundTransparency = 1
local myImg = Instance.new("ImageLabel", header)
myImg.Size = UDim2.new(0, 45, 0, 45); myImg.Position = UDim2.new(0, 15, 0, 10); myImg.Image = pImageId; myImg.BackgroundTransparency = 1; Instance.new("UICorner", myImg).CornerRadius = UDim.new(1, 0)
local title = Instance.new("TextLabel", header)
title.Size = UDim2.new(1, -80, 1, 0); title.Position = UDim2.new(0, 70, 0, 0); title.Text = "سكربت شهد shh~"; title.TextColor3 = Color3.new(1,1,1); title.TextScaled = true; title.Font = Enum.Font.FredokaOne; title.BackgroundTransparency = 1

-- مربع الكتابة
local mainInput = Instance.new("TextBox", mainFrame)
mainInput.Size = UDim2.new(0.9, 0, 0, 50); mainInput.Position = UDim2.new(0.05, 0, 0.18, 0); mainInput.PlaceholderText = "سولف هنا😇💔 "; mainInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30); mainInput.TextColor3 = Color3.new(1,1,1); mainInput.TextScaled = true; Instance.new("UICorner", mainInput)

-- الحجم وقائمة الألوان
local sizeInput = Instance.new("TextBox", mainFrame)
sizeInput.Size = UDim2.new(0.2, 0, 0, 40); sizeInput.Position = UDim2.new(0.05, 0, 0.33, 0); sizeInput.Text = "60"; sizeInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40); sizeInput.TextColor3 = Color3.new(1,1,1); Instance.new("UICorner", sizeInput)

local colorFrame = Instance.new("ScrollingFrame", mainFrame)
colorFrame.Size = UDim2.new(0.65, 0, 0, 40); colorFrame.Position = UDim2.new(0.3, 0, 0.33, 0); colorFrame.CanvasSize = UDim2.new(25, 0, 0, 0); colorFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20); colorFrame.ScrollBarThickness = 2; Instance.new("UICorner", colorFrame)

local colors = {
    -- الألوان (108 لون)
    {C = Color3.fromRGB(255, 105, 180), Tag = "rgb(255,105,180)"}, {C = Color3.fromRGB(255, 0, 0), Tag = "rgb(255,0,0)"},
    {C = Color3.fromRGB(0, 255, 0), Tag = "rgb(0,255,0)"}, {C = Color3.fromRGB(0, 0, 255), Tag = "rgb(0,0,255)"},
    {C = Color3.fromRGB(255, 255, 0), Tag = "rgb(255,255,0)"}, {C = Color3.fromRGB(0, 255, 255), Tag = "rgb(0,255,255)"},
    {C = Color3.fromRGB(255, 0, 255), Tag = "rgb(255,0,255)"}, {C = Color3.fromRGB(255, 165, 0), Tag = "rgb(255,165,0)"},
    {C = Color3.fromRGB(255, 255, 255), Tag = "rgb(255,255,255)"}, {C = Color3.fromRGB(0, 0, 0), Tag = "rgb(0,0,0)"},
    {C = Color3.fromRGB(128, 0, 0), Tag = "rgb(128,0,0)"}, {C = Color3.fromRGB(0, 128, 0), Tag = "rgb(0,128,0)"},
    {C = Color3.fromRGB(0, 0, 128), Tag = "rgb(0,0,128)"}, {C = Color3.fromRGB(128, 128, 0), Tag = "rgb(128,128,0)"},
    {C = Color3.fromRGB(128, 0, 128), Tag = "rgb(128,0,128)"}, {C = Color3.fromRGB(0, 128, 128), Tag = "rgb(0,128,128)"},
    {C = Color3.fromRGB(255, 192, 203), Tag = "rgb(255,192,203)"}, {C = Color3.fromRGB(255, 215, 0), Tag = "rgb(255,215,0)"}
}

for i = 1, 90 do
    local r, g, b = math.random(0,255), math.random(0,255), math.random(0,255)
    table.insert(colors, {C = Color3.fromRGB(r,g,b), Tag = "rgb("..r..","..g..","..b..")"})
end

local selectedColor = colors[1].Tag
for i, v in ipairs(colors) do
    local cBtn = Instance.new("TextButton", colorFrame)
    cBtn.Size = UDim2.new(0, 30, 0, 30); cBtn.Position = UDim2.new(0, (i-1)*40 + 5, 0, 5); cBtn.BackgroundColor3 = v.C; cBtn.Text = ""
    Instance.new("UICorner", cBtn).CornerRadius = UDim.new(0.3, 0)
    cBtn.MouseButton1Click:Connect(function() 
        selectedColor = v.Tag; mainStroke.Color = v.C
        if sg:FindFirstChild("MiniInputFrame") then sg.MiniInputFrame.UIStroke.Color = v.C end
    end)
end

-- قائمة الخطوط
local fontFrame = Instance.new("ScrollingFrame", mainFrame)
fontFrame.Size = UDim2.new(0.9, 0, 0, 40)
fontFrame.Position = UDim2.new(0.05, 0, 0.46, 0)
fontFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
fontFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
fontFrame.ScrollBarThickness = 2
Instance.new("UICorner", fontFrame)

local fontListLayout = Instance.new("UIListLayout", fontFrame)
fontListLayout.FillDirection = Enum.FillDirection.Horizontal
fontListLayout.SortOrder = Enum.SortOrder.LayoutOrder
fontListLayout.Padding = UDim.new(0, 8)

local fontPadding = Instance.new("UIPadding", fontFrame)
fontPadding.PaddingLeft = UDim.new(0, 5)
fontPadding.PaddingTop = UDim.new(0, 5)

local fonts = {}
for _, font in ipairs(Enum.Font:GetEnumItems()) do
    table.insert(fonts, font.Name)
end
table.sort(fonts)

local selectedFont = "FredokaOne"
for i, fName in ipairs(fonts) do
    local fBtn = Instance.new("TextButton", fontFrame)
    fBtn.Size = UDim2.new(0, 110, 0, 30)
    fBtn.BackgroundColor3 = (fName == selectedFont) and Color3.fromRGB(255, 105, 180) or Color3.fromRGB(40, 40, 40)
    fBtn.TextColor3 = Color3.new(1, 1, 1)
    fBtn.Text = fName
    fBtn.TextSize = 13
    fBtn.LayoutOrder = i
    pcall(function() fBtn.Font = Enum.Font[fName] end)
    Instance.new("UICorner", fBtn)
    
    fBtn.MouseButton1Click:Connect(function()
        selectedFont = fName
        for _, child in ipairs(fontFrame:GetChildren()) do
            if child:IsA("TextButton") then
                child.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
            end
        end
        fBtn.BackgroundColor3 = Color3.fromRGB(255, 105, 180)
    end)
end
fontFrame.CanvasSize = UDim2.new(0, #fonts * 118 + 10, 0, 0)

-- زر السبام
local spamBtn = Instance.new("TextButton", mainFrame)
spamBtn.Size = UDim2.new(0.9, 0, 0, 45); spamBtn.Position = UDim2.new(0.05, 0, 0.61, 0); spamBtn.Text = "تشغيل السبام 🚀"; spamBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40); spamBtn.TextColor3 = Color3.new(1,1,1); spamBtn.TextScaled = true; Instance.new("UICorner", spamBtn)

local function sendMessage(inputBox)
    local text = inputBox.Text; if text == "" then return end
    local size = sizeInput.Text
    local formatted = '<font face="'..selectedFont..'" color="'..selectedColor..'" size="'..size..'"><b>'..text..'</b></font>'
    
    local remote = ReplicatedStorage:FindFirstChild("ChatEvent")
    if remote then
        local args = {
            [1] = "\216\180\216\167\216\170",
            [2] = formatted
        }
        remote:FireServer(unpack(args))
        if not spamming then inputBox.Text = ""; inputBox:ReleaseFocus() end
    end
end

spamBtn.MouseButton1Click:Connect(function()
    spamming = not spamming; spamBtn.Text = spamming and "إيقاف السبام 🛑" or "تشغيل السبام 🚀"
    spamBtn.BackgroundColor3 = spamming and Color3.fromRGB(100, 0, 0) or Color3.fromRGB(40, 40, 40)
    while spamming do sendMessage(mainInput); task.wait(0.3) end
end)

-- المربع الميني
local miniFrame = Instance.new("Frame", sg)
miniFrame.Name = "MiniInputFrame"; miniFrame.Size = UDim2.new(0, 150, 0, 45); miniFrame.Position = UDim2.new(0, 10, 0.7, 0); miniFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25); miniFrame.Active = true; Instance.new("UICorner", miniFrame); makeDraggable(miniFrame)
local mStroke = Instance.new("UIStroke", miniFrame); mStroke.Color = Color3.fromRGB(255, 105, 180); mStroke.Thickness = 2
local miniInput = Instance.new("TextBox", miniFrame); miniInput.Size = UDim2.new(1, 0, 1, 0); miniInput.BackgroundTransparency = 1; miniInput.PlaceholderText = "سولف سريع🕊️ "; miniInput.TextColor3 = Color3.new(1,1,1); miniInput.TextScaled = true; miniInput.Parent = miniFrame

mainInput.FocusLost:Connect(function(e) if e then sendMessage(mainInput) end end)
miniInput.FocusLost:Connect(function(e) if e then sendMessage(miniInput) end end)

-- التحكم بالواجهة
local minBtn = Instance.new("TextButton", mainFrame); minBtn.Size = UDim2.new(0, 30, 0, 30); minBtn.Position = UDim2.new(1, -35, 0, 5); minBtn.Text = "-"; minBtn.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2); minBtn.TextColor3 = Color3.new(1,1,1); Instance.new("UICorner", minBtn)

local icon = Instance.new("ImageButton", sg)
icon.Size = UDim2.new(0, 60, 0, 60)
icon.Position = UDim2.new(0.9, 0, 0.1, 0)
icon.Image = pImageId
icon.Visible = true
Instance.new("UICorner", icon).CornerRadius = UDim.new(1, 0)

minBtn.MouseButton1Click:Connect(function() mainFrame.Visible = false end)
icon.MouseButton1Click:Connect(function() mainFrame.Visible = not mainFrame.Visible end)

StarterGui:SetCore("SendNotification", {Title = "shhode320", Text = "حيووووو السكربت اشتغلل💥😍 ", Icon = pImageId, Duration = 5})
