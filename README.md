-- CAVEIRA V9 - DELTA GOD FIX 🤡
local LP = game.Players.LocalPlayer
print("CAVEIRA INICIADO...")

-- acha o remote de trancar sozinho
local LockRemote = nil
for _,v in pairs(game:GetDescendants()) do
    if v.Name:lower():find("lock") then
        LockRemote = v
        print("Lock achado:", v:GetFullName())
    end
end

while true do
    task.wait(0.4)
    pcall(function()
        -- noclip pra entrar na base trancada
        for _,part in pairs(LP.Character:GetDescendants()) do
            if part:IsA("BasePart") then part.CanCollide = false end
        end

        -- verifica se ta segurando algo
        local segurando = false
        if LP.Character:FindFirstChildWhichIsA("Tool") then segurando = true end
        for _,m in pairs(LP.Character:GetChildren()) do
            if m:IsA("Model") and m:FindFirstChild("PricePerSecond") then segurando = true end
        end

        if segurando then
            -- VOLTA PRA TUA BASE
            for _,plot in pairs(workspace.Plots:GetChildren()) do
                if plot:FindFirstChild("Owner") and plot.Owner.Value == LP.Name then
                    LP.Character.HumanoidRootPart.CFrame = plot:GetPivot() + Vector3.new(0,8,0)
                    print("Voltando pra base!")
                end
            end
        else
            -- PROCURA O MELHOR DE 100K+
            local best, bestVal = nil, 0
            for _,plot in pairs(workspace.Plots:GetChildren()) do
                if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                    for _,clown in pairs(plot:GetChildren()) do
                        if clown:FindFirstChild("PricePerSecond") then
                            local val = clown.PricePerSecond.Value
                            if val >= 100000 and val > bestVal then
                                bestVal = val
                                best = clown
                            end
                        end
                    end
                end
            end
            
            if best then
                print("Indo roubar:", best.Name, bestVal)
                LP.Character.HumanoidRootPart.CFrame = best.PrimaryPart.CFrame + Vector3.new(0,3,0)
                task.wait(0.3)
                for _,obj in pairs(best:GetDescendants()) do
                    if obj:IsA("ProximityPrompt") then
                        obj.MaxActivationDistance = 100
                        obj.HoldDuration = 0
                        fireproximityprompt(obj)
                        print("Roubando!")
                    end
                end
            end
        end
        
        if LockRemote then LockRemote:FireServer() end
    end)
end
