-- Roba un payaso — Script Creado por MIIIGUEX - V11 GOD
local LP = game.Players.LocalPlayer
local savedPos = nil
local noclipOn = false
local autoOn = false
local Min = 100000
local Max = 500000000

-- GUI
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "MIIIGUEX_HUB"
pcall(function() game.CoreGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 310, 0, 460)
main.Position = UDim2.new(0.5, -155, 0.5, -230)
main.BackgroundColor3 = Color3.fromRGB(18,18,22)
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,14)

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1,0,0,60)
title.Text = "Roba un payaso\n— Script Creado por MIIIGUEX"
title.TextColor3 = Color3.new(1,1,1)
title.TextScaled = true
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold

local function CreateBtn(text, y, color)
    local b = Instance.new("TextButton", main)
    b.Size = UDim2.new(0.9,0,0,42)
    b.Position = UDim2.new(0.05,0,0,y)
    b.Text = text
    b.TextColor3 = Color3.new(1,1,1)
    b.BackgroundColor3 = color or Color3.fromRGB(35,40,55)
    b.Font = Enum.Font.GothamMedium
    b.TextSize = 15
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,10)
    return b
end

local btnSave = CreateBtn("Guardar Posición", 70)
local btnTP = CreateBtn("TP Guardado", 120)
local btnNoclip = CreateBtn("NoClip: OFF", 170)
local btnAuto = CreateBtn("🤡 AUTO ROBAR 100K-500M: OFF", 220, Color3.fromRGB(80,30,30))

local visualLabel = Instance.new("TextLabel", main)
visualLabel.Position = UDim2.new(0.05,0,0,275)
visualLabel.Size = UDim2.new(0.9,0,0,25)
visualLabel.Text = "Visuales (ESP)"
visualLabel.TextColor3 = Color3.new(1,1,1)
visualLabel.BackgroundTransparency = 1
visualLabel.TextXAlignment = Enum.TextXAlignment.Left
visualLabel.Font = Enum.Font.GothamBold
visualLabel.TextSize = 15

local btnESP = CreateBtn("ESP Nombres: OFF", 305)
local btnESPValor = CreateBtn("ESP Valores: OFF", 355)
local btnClose = CreateBtn("Cerrar", 405, Color3.fromRGB(45,45,45))

-- LÓGICA
btnSave.MouseButton1Click:Connect(function()
    savedPos = LP.Character.HumanoidRootPart.CFrame
    btnSave.Text = "✅ Posición Guardada!"
    task.wait(1)
    btnSave.Text = "Guardar Posición"
end)

btnTP.MouseButton1Click:Connect(function()
    if savedPos then LP.Character.HumanoidRootPart.CFrame = savedPos end
end)

btnNoclip.MouseButton1Click:Connect(function()
    noclipOn = not noclipOn
    btnNoclip.Text = noclipOn and "NoClip: ON" or "NoClip: OFF"
    btnNoclip.BackgroundColor3 = noclipOn and Color3.fromRGB(0,150,80) or Color3.fromRGB(35,40,55)
end)

-- AUTO ROUBO 100K A 500M
function Segurando()
    if LP.Character:FindFirstChildWhichIsA("Tool") then return true end
    for _,v in pairs(LP.Character:GetChildren()) do
        if v:IsA("Model") and v:FindFirstChild("PricePerSecond") then return true end
    end
    return false
end

btnAuto.MouseButton1Click:Connect(function()
    autoOn = not autoOn
    btnAuto.Text = autoOn and "🤡 AUTO ROBAR: ON" or "🤡 AUTO ROBAR 100K-500M: OFF"
    btnAuto.BackgroundColor3 = autoOn and Color3.fromRGB(0,150,80) or Color3.fromRGB(80,30,30)
end)

-- Loop do auto roubo
task.spawn(function()
    while true do
        task.wait(0.35)
        if autoOn then
            pcall(function()
                -- noclip auto
                for _,p in pairs(LP.Character:GetDescendants()) do
                    if p:IsA("BasePart") then p.CanCollide = false end
                end

                if Segurando() then
                    for _,plot in pairs(workspace.Plots:GetChildren()) do
                        if plot:FindFirstChild("Owner") and plot.Owner.Value == LP.Name then
                            LP.Character.HumanoidRootPart.CFrame = plot:GetPivot() + Vector3.new(0,8,0)
                        end
                    end
                else
                    local melhor, valorMelhor = nil, 0
                    for _,plot in pairs(workspace.Plots:GetChildren()) do
                        if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                            for _,clown in pairs(plot:GetChildren()) do
                                if clown:FindFirstChild("PricePerSecond") and clown:FindFirstChild("PrimaryPart") then
                                    local val = clown.PricePerSecond.Value
                                    if val >= Min and val <= Max and val > valorMelhor then
                                        valorMelhor = val
                                        melhor = clown
                                    end
                                end
                            end
                        end
                    end
                    if melhor then
                        LP.Character.HumanoidRootPart.CFrame = melhor.PrimaryPart.CFrame + Vector3.new(0,3,0)
                        task.wait(0.25)
                        for _,obj in pairs(melhor:GetDescendants()) do
                            if obj:IsA("ProximityPrompt") then
                                obj.MaxActivationDistance = 100
                                obj.HoldDuration = 0
                                fireproximityprompt(obj)
                            end
                        end
                    end
                end
            end)
        end
    end
end)

game:GetService("RunService").Stepped:Connect(function()
    if noclipOn or autoOn then
        for _,v in pairs(LP.Character:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = false end
        end
    end
end)

btnClose.MouseButton1Click:Connect(function() gui:Destroy() end)

game.StarterGui:SetCore("SendNotification", {Title="MIIIGUEX", Text="Script Cargado! Auto 100K-500M pronto", Duration=3})
