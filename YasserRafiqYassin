local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()

local Speed = 80
local RunSpeed = 16

local Flying = false
local Noclip = false
local ESPEnabled = false
local TeleportEnabled = false
local Minimized = false

local Keys = {}
local OriginalCollision = {}
local ESPObjects = {}

local NormalSize = UDim2.fromOffset(230, 375)

-- GUI

local Gui = Instance.new("ScreenGui")
Gui.Name = "YasserRafiqYassin"
Gui.ResetOnSpawn = false
Gui.Parent = Player:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Size = NormalSize
Frame.Position = UDim2.new(0, 30, 0.4, 0)
Frame.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Parent = Gui

Instance.new("UICorner", Frame).CornerRadius = UDim.new(0, 12)

local Stroke = Instance.new("UIStroke")
Stroke.Color = Color3.fromRGB(55, 55, 55)
Stroke.Parent = Frame

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 38)
Title.BackgroundTransparency = 1
Title.Text = "Yasser Rafiq Yassin"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 15
Title.Parent = Frame

local function CreateButton(Text, X, Y, Width, Height)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.fromOffset(Width or 40, Height or 30)
    Button.Position = UDim2.fromOffset(X, Y)
    Button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    Button.Text = Text
    Button.TextColor3 = Color3.new(1, 1, 1)
    Button.Font = Enum.Font.GothamBold
    Button.TextSize = 13
    Button.Parent = Frame

    Instance.new("UICorner", Button).CornerRadius = UDim.new(0, 6)

    return Button
end

-- MINIMIZE

local MinimizeButton = CreateButton("-", 5, 5, 28, 28)
MinimizeButton.ZIndex = 5

local function SetMinimized(State)
    Minimized = State

    for _, Object in ipairs(Frame:GetChildren()) do
        if Object:IsA("GuiObject") and Object ~= MinimizeButton then
            Object.Visible = not Minimized
        end
    end

    Frame.Size = Minimized and UDim2.fromOffset(40, 40) or NormalSize
    MinimizeButton.Text = Minimized and "+" or "-"
end

MinimizeButton.MouseButton1Click:Connect(function()
    SetMinimized(not Minimized)
end)

-- DRAG

local Dragging = false
local DragStart
local StartPosition

Title.InputBegan:Connect(function(Input)
    if Input.UserInputType == Enum.UserInputType.MouseButton1 then
        Dragging = true
        DragStart = Input.Position
        StartPosition = Frame.Position
    end
end)

UIS.InputChanged:Connect(function(Input)
    if Dragging and Input.UserInputType == Enum.UserInputType.MouseMovement then
        local Delta = Input.Position - DragStart

        Frame.Position = UDim2.new(
            StartPosition.X.Scale,
            StartPosition.X.Offset + Delta.X,
            StartPosition.Y.Scale,
            StartPosition.Y.Offset + Delta.Y
        )
    end
end)

UIS.InputEnded:Connect(function(Input)
    if Input.UserInputType == Enum.UserInputType.MouseButton1 then
        Dragging = false
    end
end)

-- FLY

local FlyButton = CreateButton("FLY: OFF", 10, 45, 210, 32)

local SpeedLabel = Instance.new("TextLabel")
SpeedLabel.Size = UDim2.fromOffset(125, 30)
SpeedLabel.Position = UDim2.fromOffset(10, 82)
SpeedLabel.BackgroundTransparency = 1
SpeedLabel.Text = "FLY SPEED: 80"
SpeedLabel.TextColor3 = Color3.new(1, 1, 1)
SpeedLabel.Font = Enum.Font.GothamBold
SpeedLabel.TextSize = 12
SpeedLabel.Parent = Frame

local Minus = CreateButton("-", 145, 82, 35)
local Plus = CreateButton("+", 190, 82, 30)

Minus.MouseButton1Click:Connect(function()
    Speed = math.max(10, Speed - 10)
    SpeedLabel.Text = "FLY SPEED: " .. Speed
end)

Plus.MouseButton1Click:Connect(function()
    Speed = math.min(500, Speed + 10)
    SpeedLabel.Text = "FLY SPEED: " .. Speed
end)

-- NOCLIP

local NoclipButton = CreateButton("NOCLIP: OFF", 10, 120, 210, 30)

local function RestoreCollision()
    for Part, Value in pairs(OriginalCollision) do
        if Part and Part.Parent then
            Part.CanCollide = Value
        end
    end

    table.clear(OriginalCollision)
end

local NoclipConnection

local function SetNoclip(State)
    Noclip = State

    NoclipButton.Text = Noclip and "NOCLIP: ON" or "NOCLIP: OFF"

    if NoclipConnection then
        NoclipConnection:Disconnect()
        NoclipConnection = nil
    end

    if Noclip then
        NoclipConnection = RunService.Stepped:Connect(function()
            local Character = Player.Character
            if not Character then return end

            for _, Part in ipairs(Character:GetDescendants()) do
                if Part:IsA("BasePart") then
                    if OriginalCollision[Part] == nil then
                        OriginalCollision[Part] = Part.CanCollide
                    end

                    Part.CanCollide = false
                end
            end
        end)
    else
        RestoreCollision()
    end
end

NoclipButton.MouseButton1Click:Connect(function()
    SetNoclip(not Noclip)
end)

-- RUN SPEED

local RunLabel = Instance.new("TextLabel")
RunLabel.Size = UDim2.fromOffset(125, 30)
RunLabel.Position = UDim2.fromOffset(10, 157)
RunLabel.BackgroundTransparency = 1
RunLabel.Text = "RUN SPEED: 16"
RunLabel.TextColor3 = Color3.new(1, 1, 1)
RunLabel.Font = Enum.Font.GothamBold
RunLabel.TextSize = 12
RunLabel.Parent = Frame

local RunMinus = CreateButton("-", 145, 157, 35)
local RunPlus = CreateButton("+", 190, 157, 30)

local function ApplyRunSpeed()
    local Character = Player.Character
    local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")

    if Humanoid then
        Humanoid.WalkSpeed = RunSpeed
    end
end

RunMinus.MouseButton1Click:Connect(function()
    RunSpeed = math.max(16, RunSpeed - 5)
    RunLabel.Text = "RUN SPEED: " .. RunSpeed
    ApplyRunSpeed()
end)

RunPlus.MouseButton1Click:Connect(function()
    RunSpeed = math.min(200, RunSpeed + 5)
    RunLabel.Text = "RUN SPEED: " .. RunSpeed
    ApplyRunSpeed()
end)

-- ESP PLAYER

local ESPButton = CreateButton("ESP PLAYER: OFF", 10, 195, 210, 30)

local function RemoveESP(Target)
    if ESPObjects[Target] then
        ESPObjects[Target]:Destroy()
        ESPObjects[Target] = nil
    end
end

local function AddESP(Target)
    if Target == Player or not ESPEnabled then return end

    RemoveESP(Target)

    local Character = Target.Character
    local Head = Character and Character:FindFirstChild("Head")

    if not Head then return end

    local Billboard = Instance.new("BillboardGui")
    Billboard.Name = "YasserESP"
    Billboard.Adornee = Head
    Billboard.Size = UDim2.fromOffset(14, 14)
    Billboard.StudsOffset = Vector3.new(0, 2, 0)
    Billboard.AlwaysOnTop = true
    Billboard.Parent = Gui

    local Dot = Instance.new("Frame")
    Dot.Size = UDim2.fromScale(1, 1)
    Dot.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    Dot.BorderSizePixel = 0
    Dot.Parent = Billboard

    Instance.new("UICorner", Dot).CornerRadius = UDim.new(1, 0)

    ESPObjects[Target] = Billboard
end

local function UpdateESP()
    for Target in pairs(ESPObjects) do
        RemoveESP(Target)
    end

    if ESPEnabled then
        for _, Target in ipairs(Players:GetPlayers()) do
            AddESP(Target)
        end
    end
end

ESPButton.MouseButton1Click:Connect(function()
    ESPEnabled = not ESPEnabled
    ESPButton.Text = ESPEnabled and "ESP PLAYER: ON" or "ESP PLAYER: OFF"
    UpdateESP()
end)

local function ConnectPlayer(Target)
    if Target == Player then return end

    Target.CharacterAdded:Connect(function()
        task.wait(1)
        AddESP(Target)
    end)

    if Target.Character then
        AddESP(Target)
    end
end

Players.PlayerAdded:Connect(ConnectPlayer)

Players.PlayerRemoving:Connect(function(Target)
    RemoveESP(Target)
end)

for _, Target in ipairs(Players:GetPlayers()) do
    ConnectPlayer(Target)
end

-- TELEPORT TOOL
-- LocalScript teleport is only a local character change.

local TeleportButton = CreateButton("TELEPORT TOOL: OFF", 10, 232, 210, 30)

local TeleportTool

local function GiveTeleportTool()
    if TeleportTool then return end

    TeleportTool = Instance.new("Tool")
    TeleportTool.Name = "Teleport Tool"
    TeleportTool.RequiresHandle = false
    TeleportTool.CanBeDropped = false
    TeleportTool.Parent = Player.Backpack

    TeleportTool.Activated:Connect(function()
        local Character = Player.Character
        local Root = Character and Character:FindFirstChild("HumanoidRootPart")

        if Root and Mouse.Hit then
            Root.CFrame = CFrame.new(Mouse.Hit.Position + Vector3.new(0, 4, 0))
        end
    end)
end

local function RemoveTeleportTool()
    if TeleportTool then
        TeleportTool:Destroy()
        TeleportTool = nil
    end
end

TeleportButton.MouseButton1Click:Connect(function()
    TeleportEnabled = not TeleportEnabled

    TeleportButton.Text = TeleportEnabled
        and "TELEPORT TOOL: ON"
        or "TELEPORT TOOL: OFF"

    if TeleportEnabled then
        GiveTeleportTool()
    else
        RemoveTeleportTool()
    end
end)

-- DANCES

local DanceButton = CreateButton("DANCES: OPEN", 10, 269, 210, 30)

local DanceFrame = Instance.new("Frame")
DanceFrame.Size = UDim2.fromOffset(210, 245)
DanceFrame.Position = UDim2.new(0, 250, 0.4, 0)
DanceFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
DanceFrame.Visible = false
DanceFrame.Active = true
DanceFrame.Parent = Gui

Instance.new("UICorner", DanceFrame).CornerRadius = UDim.new(0, 12)

local DanceStroke = Instance.new("UIStroke")
DanceStroke.Color = Color3.fromRGB(65, 65, 65)
DanceStroke.Parent = DanceFrame

local DanceTitle = Instance.new("TextLabel")
DanceTitle.Size = UDim2.new(1, 0, 0, 35)
DanceTitle.BackgroundTransparency = 1
DanceTitle.Text = "YASSER DANCES"
DanceTitle.TextColor3 = Color3.new(1, 1, 1)
DanceTitle.Font = Enum.Font.GothamBold
DanceTitle.TextSize = 14
DanceTitle.Parent = DanceFrame

local DanceAnimations = {
    ["Dance 1"] = "507771019",
    ["Dance 2"] = "507776043",
    ["Dance 3"] = "507777268",
    ["Cheer"] = "507770677",
    ["Wave"] = "507770239"
}

local CurrentDance

local function StopDance()
    if CurrentDance then
        CurrentDance:Stop(0.2)
        CurrentDance:Destroy()
        CurrentDance = nil
    end
end

local function PlayDance(Name)
    local Character = Player.Character
    local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")

    if not Humanoid then return end

    local Animator = Humanoid:FindFirstChildOfClass("Animator")

    if not Animator then
        Animator = Instance.new("Animator")
        Animator.Parent = Humanoid
    end

    StopDance()

    local Animation = Instance.new("Animation")
    Animation.AnimationId = "rbxassetid://" .. DanceAnimations[Name]

    local Success, Track = pcall(function()
        return Animator:LoadAnimation(Animation)
    end)

    if Success and Track then
        Track.Looped = true
        Track:Play()
        CurrentDance = Track
    end
end

local DanceNames = {
    "Dance 1",
    "Dance 2",
    "Dance 3",
    "Cheer",
    "Wave"
}

for Index, Name in ipairs(DanceNames) do
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.fromOffset(190, 30)
    Button.Position = UDim2.fromOffset(10, 38 + (Index - 1) * 35)
    Button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    Button.Text = Name
    Button.TextColor3 = Color3.new(1, 1, 1)
    Button.Font = Enum.Font.GothamBold
    Button.TextSize = 12
    Button.Parent = DanceFrame

    Instance.new("UICorner", Button).CornerRadius = UDim.new(0, 6)

    Button.MouseButton1Click:Connect(function()
        PlayDance(Name)
    end)
end

local StopButton = Instance.new("TextButton")
StopButton.Size = UDim2.fromOffset(190, 30)
StopButton.Position = UDim2.fromOffset(10, 215)
StopButton.BackgroundColor3 = Color3.fromRGB(130, 30, 30)
StopButton.Text = "STOP DANCE"
StopButton.TextColor3 = Color3.new(1, 1, 1)
StopButton.Font = Enum.Font.GothamBold
StopButton.TextSize = 12
StopButton.Parent = DanceFrame

Instance.new("UICorner", StopButton).CornerRadius = UDim.new(0, 6)

StopButton.MouseButton1Click:Connect(StopDance)

DanceButton.MouseButton1Click:Connect(function()
    DanceFrame.Visible = not DanceFrame.Visible
end)

-- DANCE WINDOW DRAG

local DanceDragging = false
local DanceDragStart
local DanceStartPosition

DanceTitle.InputBegan:Connect(function(Input)
    if Input.UserInputType == Enum.UserInputType.MouseButton1 then
        DanceDragging = true
        DanceDragStart = Input.Position
        DanceStartPosition = DanceFrame.Position
    end
end)

UIS.InputChanged:Connect(function(Input)
    if DanceDragging and Input.UserInputType == Enum.UserInputType.MouseMovement then
        local Delta = Input.Position - DanceDragStart

        DanceFrame.Position = UDim2.new(
            DanceStartPosition.X.Scale,
            DanceStartPosition.X.Offset + Delta.X,
            DanceStartPosition.Y.Scale,
            DanceStartPosition.Y.Offset + Delta.Y
        )
    end
end)

UIS.InputEnded:Connect(function(Input)
    if Input.UserInputType == Enum.UserInputType.MouseButton1 then
        DanceDragging = false
    end
end)

-- KEYBOARD

UIS.InputBegan:Connect(function(Input, Processed)
    if not Processed then
        Keys[Input.KeyCode] = true
    end
end)

UIS.InputEnded:Connect(function(Input)
    Keys[Input.KeyCode] = false
end)

-- FLY SYSTEM

local Velocity, Attachment, Align
local FlyConnection

local function StopFlying()
    Flying = false

    if FlyConnection then
        FlyConnection:Disconnect()
        FlyConnection = nil
    end

    if Velocity then Velocity:Destroy() Velocity = nil end
    if Align then Align:Destroy() Align = nil end
    if Attachment then Attachment:Destroy() Attachment = nil end

    local Character = Player.Character
    local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")

    if Humanoid then
        Humanoid.PlatformStand = false
        Humanoid.AutoRotate = true
        Humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
    end

    FlyButton.Text = "FLY: OFF"
end

local function StartFlying()
    local Character = Player.Character
    local Root = Character and Character:FindFirstChild("HumanoidRootPart")
    local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")

    if not Root or not Humanoid or Flying then return end

    Flying = true
    Humanoid.PlatformStand = true
    Humanoid.AutoRotate = false

    Attachment = Instance.new("Attachment", Root)

    Velocity = Instance.new("LinearVelocity")
    Velocity.Attachment0 = Attachment
    Velocity.RelativeTo = Enum.ActuatorRelativeTo.World
    Velocity.MaxForce = 1000000
    Velocity.VectorVelocity = Vector3.zero
    Velocity.Parent = Root

    Align = Instance.new("AlignOrientation")
    Align.Attachment0 = Attachment
    Align.Mode = Enum.OrientationAlignmentMode.OneAttachment
    Align.MaxTorque = 1000000
    Align.Responsiveness = 15
    Align.Parent = Root

    FlyConnection = RunService.Heartbeat:Connect(function()
        if not Root.Parent or not Humanoid.Parent then
            StopFlying()
            return
        end

        local Camera = workspace.CurrentCamera
        if not Camera then return end

        local Direction = Vector3.zero

        if Keys[Enum.KeyCode.W] then Direction += Camera.CFrame.LookVector end
        if Keys[Enum.KeyCode.S] then Direction -= Camera.CFrame.LookVector end
        if Keys[Enum.KeyCode.A] then Direction -= Camera.CFrame.RightVector end
        if Keys[Enum.KeyCode.D] then Direction += Camera.CFrame.RightVector end
        if Keys[Enum.KeyCode.Space] then Direction += Vector3.yAxis end
        if Keys[Enum.KeyCode.LeftControl] then Direction -= Vector3.yAxis end

        Velocity.VectorVelocity = Direction.Magnitude > 0
            and Direction.Unit * Speed
            or Vector3.zero

        Align.CFrame = CFrame.lookAt(
            Root.Position,
            Root.Position + Camera.CFrame.LookVector
        )
    end)

    FlyButton.Text = "FLY: ON"
end

FlyButton.MouseButton1Click:Connect(function()
    if Flying then
        StopFlying()
    else
        StartFlying()
    end
end)

-- RESPAWN

Player.CharacterAdded:Connect(function()
    if Flying then
        StopFlying()
    end

    StopDance()
    RestoreCollision()

    task.wait(0.5)

    ApplyRunSpeed()

    if Noclip then
        SetNoclip(true)
    end

    if TeleportEnabled then
        RemoveTeleportTool()
        GiveTeleportTool()
    end

    if ESPEnabled then
        UpdateESP()
    end
end)
