-- CAVEIRA V10 DO ZERO - AUTO VAR 100K A 500M 🤡
local LP = game.Players.LocalPlayer
local Min = 100000 -- 100K
local Max = 500000000 -- 500M

print("CAVEIRA V10 ATIVADO - 100K A 500M")

-- Função pra ver se tá segurando palhaço
function Segurando()
    if LP.Character:FindFirstChildWhichIsA("Tool") then return true end
    for _,v in pairs(LP.Character:GetChildren()) do
        if v:IsA("Model") and v:FindFirstChild("PricePerSecond") then return true end
    end
    return false
end

-- Loop infinito
while true do
    task.wait(0.3)
    pcall(function()
        -- NOCLIP pra entrar na base trancada
        for _,p in pairs(LP.Character:GetDescendants()) do
            if p:IsA("BasePart") then p.CanCollide = false end
        end

        if Segurando() then
            -- AUTOVAR: TEM PALHAÇO NA MÃO = VOLTA PRA BASE
            for _,plot in pairs(workspace.Plots:GetChildren()) do
                if plot:FindFirstChild("Owner") and plot.Owner.Value == LP.Name then
                    LP.Character.HumanoidRootPart.CFrame = plot:GetPivot() + Vector3.new(0,7,0)
                end
            end
        else
            -- PROCURA MELHOR PALHAÇO ENTRE 100K E 500M
            local melhor, valorMelhor = nil, 0
            
            for _,plot in pairs(workspace.Plots:GetChildren()) do
                if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                    for _,clown in pairs(plot:GetChildren()) do
                        if clown:FindFirstChild("PricePerSecond") and clown:FindFirstChild("PrimaryPart") then
                            local val = clown.PricePerSecond.Value
                            -- SÓ DE 100K A 500M
                            if val >= Min and val <= Max and val > valorMelhor then
                                valorMelhor = val
                                melhor = clown
                            end
                        end
                    end
                end
            end

            if melhor then
                -- VAI ATÉ ELE E ROUBA
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
