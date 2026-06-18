local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

local gui = Instance.new("ScreenGui")
gui.Parent = PlayerGui
gui.ResetOnSpawn = false

local botao = Instance.new("TextButton")
botao.Parent = gui
botao.Size = UDim2.new(0, 200, 0, 60)
botao.Position = UDim2.new(0.5, -100, 0.8, 0)
botao.Text = "ABRIR PORTA"
botao.TextScaled = true
botao.BackgroundColor3 = Color3.fromRGB(0,170,0)
botao.TextColor3 = Color3.new(1,1,1)

local Remote

pcall(function()
    Remote = require(
        ReplicatedStorage:WaitForChild("Packages"):WaitForChild("Remotes")
    ).Event("OpenDoor")
end)

local function abrirPorta()
    if Remote then
        pcall(function()
            Remote:FireServer()
        end)
    else
        warn("Remote não encontrado")
    end
end

botao.MouseButton1Click:Connect(abrirPorta)

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end

    if input.KeyCode == Enum.KeyCode.Semicolon then
        abrirPorta()
    end
end)
