-- MIIIGUEX MENU FIX - DELTA 2025
local LP = game.Players.LocalPlayer
local PlayerGui = LP:WaitForChild("PlayerGui")

pcall(function() PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)
pcall(function() game.CoreGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)

local gui = Instance.new("ScreenGui")
gui.Name = "MIIIGUEX_HUB"
gui.ResetOnSpawn = false
gui.Parent = PlayerGui

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 300, 0, 280)
main.Position = UDim2.new(0.5, -150, 0.5, -140)
main.BackgroundColor3 = Color3.fromRGB(25,25,30)
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,12)

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1,0,0,50)
title.Text = "Roba un payaso — Creado por MIIIGUEX"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 14

local function Btn(text, y, col)
    local b = Instance.new("TextButton", main)
    b.Size = UDim2.new(0.9,0,0,40)
    b.Position = UDim2.new(0.05,0,0,y)
    b.Text = text
    b.BackgroundColor3 = col or Color3.fromRGB(45,50,70)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.Gotham
    b.TextSize = 14
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,8)
    return b
end

local b1 = Btn("Guardar Posición", 60)
local b2 = Btn("TP Guardado", 105)
local b3 = Btn("NoClip: OFF", 150)
local b4 = Btn("🤡 AUTO 100K-500M: OFF", 195, Color3.fromRGB(120,40,40))
local b5 = Btn("Cerrar Menu", 240, Color3.fromRGB(60,60,60))

local savedPos = nil
local noclip = false
local auto = false

b1.MouseButton1Click:Connect(function() savedPos = LP.Character.HumanoidRootPart.CFrame end)
b2.MouseButton1Click:Connect(function() if savedPos then LP.Character.HumanoidRootPart.CFrame = savedPos end end)
b3.MouseButton1Click:Connect(function() noclip = not noclip b3.Text = noclip and "NoClip: ON" or "NoClip: OFF" end)
b4.MouseButton1Click:Connect(function() 
    auto = not auto 
    b4.Text = auto and "🤡 AUTO: ON" or "🤡 AUTO 100K-500M: OFF"
    b4.BackgroundColor3 = auto and Color3.fromRGB(0,180,80) or Color3.fromRGB(120,40,40)
end)
b5.MouseButton1Click:Connect(function() gui:Destroy() end)

-- loop noclip + auto
game:GetService("RunService").Stepped:Connect(function()
    if noclip or auto then
        for _,v in pairs(LP.Character:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end
    end
end)

task.spawn(function()
    while true do
        task.wait(0.4)
        if auto then
            pcall(function()
                local segurando = LP.Character:FindFirstChildWhichIsA("Tool") ~= nil
                if segurando then
                    for _,plot in pairs(workspace.Plots:GetChildren()) do
                        if plot:FindFirstChild("Owner") and plot.Owner.Value == LP.Name then
                            LP.Character.HumanoidRootPart.CFrame = plot:GetPivot() + Vector3.new(0,7,0)
                        end
                    end
                else
                    local melhor, valM = nil, 0
                    for _,plot in pairs(workspace.Plots:GetChildren()) do
                        if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                            for _,c in pairs(plot:GetChildren()) do
                                if c:FindFirstChild("PricePerSecond") and c:FindFirstChild("PrimaryPart") then
                                    local v = c.PricePerSecond.Value
                                    if v >= 100000 and v <= 500000000 and v > valM then
                                        valM = v melhor = c
                                    end
                                end
                            end
                        end
                    end
                    if melhor then
                        LP.Character.HumanoidRootPart.CFrame = melhor.PrimaryPart.CFrame + Vector3.new(0,2,0)
                        task.wait(0.2)
                        for _,p in pairs(melhor:GetDescendants()) do if p:IsA("ProximityPrompt") then fireproximityprompt(p) end end
                    end
                end
            end)
        end
    end
end)

game.StarterGui:SetCore("SendNotification", {Title="MIIIGUEX", Text="Menu carregado!", Duration=3})
