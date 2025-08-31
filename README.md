local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local UICorner = Instance.new("UICorner")
local Title = Instance.new("TextLabel")
local Icon = Instance.new("ImageLabel")

ScreenGui.Parent = game.CoreGui
ScreenGui.Name = "GreatHub"

MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.Position = UDim2.new(0.3, 0, 0.3, 0)
MainFrame.Size = UDim2.new(0, 400, 0, 300)

UICorner.CornerRadius = UDim.new(0, 12)
UICorner.Parent = MainFrame

Title.Parent = MainFrame
Title.Text = "Great Hub"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.fromRGB(0, 255, 0)
Title.TextScaled = true
Title.Font = Enum.Font.GothamBold

Icon.Parent = MainFrame
Icon.Size = UDim2.new(0, 40, 0, 40)
Icon.Position = UDim2.new(0, 10, 0, 10)
Icon.BackgroundTransparency = 1
Icon.Image = "rbxassetid://6023426926"

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

local function criarBotao(nome, pos, func)
    local btn = Instance.new("TextButton")
    btn.Parent = MainFrame
    btn.Size = UDim2.new(0, 150, 0, 40)
    btn.Position = pos
    btn.Text = nome
    btn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.GothamBold
    btn.TextScaled = true
    local ui = Instance.new("UICorner")
    ui.CornerRadius = UDim.new(0, 8)
    ui.Parent = btn
    btn.MouseButton1Click:Connect(func)
end

local flying = false
local function fly()
    flying = not flying
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if flying then
        humanoid.PlatformStand = true
        local bp = Instance.new("BodyPosition", character.PrimaryPart)
        local bg = Instance.new("BodyGyro", character.PrimaryPart)
        bp.MaxForce = Vector3.new(4000,4000,4000)
        bg.MaxTorque = Vector3.new(4000,4000,4000)
        game:GetService("RunService").RenderStepped:Connect(function()
            if flying then
                bp.Position = bp.Position + (character.PrimaryPart.CFrame.LookVector * 2)
            end
        end)
    else
        humanoid.PlatformStand = false
        if character.PrimaryPart:FindFirstChild("BodyPosition") then
            character.PrimaryPart.BodyPosition:Destroy()
        end
        if character.PrimaryPart:FindFirstChild("BodyGyro") then
            character.PrimaryPart.BodyGyro:Destroy()
        end
    end
end

local speedOn = false
local function speed()
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    speedOn = not speedOn
    humanoid.WalkSpeed = speedOn and 60 or 16
end

local jumpOn = false
local function superJump()
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    jumpOn = not jumpOn
    humanoid.JumpPower = jumpOn and 150 or 50
end

local function tp()
    local mouse = player:GetMouse()
    if mouse.Target then
        character:MoveTo(mouse.Hit.p + Vector3.new(0,3,0))
    end
end

criarBotao("Voar", UDim2.new(0.05, 0, 0.2, 0), fly)
criarBotao("Velocidade", UDim2.new(0.05, 0, 0.35, 0), speed)
criarBotao("Super Pulo", UDim2.new(0.05, 0, 0.5, 0), superJump)
criarBotao("Teleporte", UDim2.new(0.05, 0, 0.65, 0), tp)
