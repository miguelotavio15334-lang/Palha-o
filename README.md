-- CAVEIRA HUB V8 - ENTRA, ROUBA E VOLTA SOZINHO 🤡 [GOD]
local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/GRPGaming/Key-System/refs/heads/Xycer-Hub-Source/source"))()
local Window = OrionLib:MakeWindow({Name = "CAVEIRA V8 - AUTO TUDO 🤡", HidePremium = false, SaveConfig = true, ConfigFolder = "CaveiraV8"})

local LP = game.Players.LocalPlayer
local RS = game:GetService("ReplicatedStorage")

function GetBest()
    local best, val = nil, 0
    for _,plot in pairs(workspace.Plots:GetChildren()) do
        if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
            for _,clown in pairs(plot:GetChildren()) do
                if clown:FindFirstChild("PricePerSecond") and clown:FindFirstChild("PrimaryPart") then
                    if clown.PricePerSecond.Value > val then
                        val = clown.PricePerSecond.Value
                        best = clown
                    end
                end
            end
        end
    end
    return best, val
end

function GetMyPlot()
    for _,plot in pairs(workspace.Plots:GetChildren()) do
        if plot:FindFirstChild("Owner") and plot.Owner.Value == LP.Name then return plot end
    end
    return nil
end

function HasClown()
    if LP.Character:FindFirstChildWhichIsA("Tool") then return true end
    for _,v in pairs(LP.Character:GetChildren()) do if v:IsA("Model") and v:FindFirstChild("PricePerSecond") then return true end end
    return false
end

function NoclipOn()
    for _,v in pairs(LP.Character:GetDescendants()) do
        if v:IsA("BasePart") then v.CanCollide = false end
    end
end

local Tab = Window:MakeTab({Name = "Auto Tudo", Icon = "rbxassetid://4483345998", PremiumOnly = false})

Tab:AddSection({Name = "Entra > Rouba > Volta"})

Tab:AddToggle({
    Name = "AUTO: Entrar na Base, Roubar e Voltar [100K+]",
    Default = false,
    Callback = function(v)
        _G.AutoTudo = v
        while _G.AutoTudo do
            task.wait(0.2)
            pcall(function()
                NoclipOn()
                
                -- 1. SE TÁ SEGURANDO, VOLTA PRA BASE
                if HasClown() then
                    local my = GetMyPlot()
                    if my then
                        NoclipOn()
                        LP.Character.HumanoidRootPart.CFrame = my:GetPivot() + Vector3.new(0,5,0)
                        task.wait(1.5)
                    end
                else
                    -- 2. SE NÃO TÁ, VAI ROUBAR O MELHOR
                    local best, val = GetBest()
                    if best and val >= 100000 then
                        NoclipOn()
                        -- entra na base do cara mesmo trancada
                        LP.Character.HumanoidRootPart.CFrame = best.PrimaryPart.CFrame + Vector3.new(0,3,0)
                        task.wait(0.2)
                        local prompt = best:FindFirstChildWhichIsA("ProximityPrompt", true)
                        if prompt then
                            prompt.MaxActivationDistance = 50
                            fireproximityprompt(prompt)
                        end
                    end
                end
                
                -- sempre tranca a tua
                RS.Remotes.LockBase:FireServer()
            end)
        end
    end
})

Tab:AddToggle({
    Name = "Auto Roubar MELHOR do Server (ignora valor)",
    Default = false,
    Callback = function(v)
        _G.BestOnly = v
        while _G.BestOnly do
            task.wait(0.2)
            pcall(function()
                NoclipOn()
                if HasClown() then
                    local my = GetMyPlot()
                    if my then LP.Character.HumanoidRootPart.CFrame = my:GetPivot() + Vector3.new(0,5,0) end
                else
                    local best, val = GetBest()
                    if best then
                        LP.Character.HumanoidRootPart.CFrame = best.PrimaryPart.CFrame + Vector3.new(0,3,0)
                        task.wait(0.2)
                        fireproximityprompt(best:FindFirstChildWhichIsA("ProximityPrompt", true))
                    end
                end
            end)
        end
    end
})

Tab:AddSlider({Name = "Minimo pra roubar", Min = 50000, Max = 1000000, Default = 100000, Increment = 50000, ValueName = "/s", Callback = function(v) _G.MinCustom = v end})

Tab:AddButton({Name = "Ver Melhor Atual", Callback = function()
    local b,v = GetBest()
    if b then OrionLib:MakeNotification({Name = "MELHOR", Content = b.Name.." - "..v.."/s", Time = 4}) end
end})

OrionLib:Init()
OrionLib:MakeNotification({Name = "CAVEIRA V8", Content = "Ativa o primeiro toggle! Entra, rouba e volta sozinho 🤡", Time = 5})
