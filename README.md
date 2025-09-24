-- @Nova.Br Auto Float v2 GUI (UI Moderna + Plataforma Aprimorada + ESP Colorido)
local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.Name = "NovaBrUI"
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Main Frame com gradiente
local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 220, 0, 85)
main.Position = UDim2.new(0.5, -110, 0.1, 0)
main.BackgroundColor3 = Color3.fromRGB(15, 15, 25)
main.BackgroundTransparency = 0.05
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true
main.ClipsDescendants = true

-- Gradiente de fundo
local gradient = Instance.new("UIGradient", main)
gradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(25, 20, 45)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(45, 25, 65))
}
gradient.Rotation = 45

-- Corner arredondado
local corner = Instance.new("UICorner", main)
corner.CornerRadius = UDim.new(0, 15)

-- Borda brilhante
local stroke = Instance.new("UIStroke", main)
stroke.Thickness = 2
stroke.Transparency = 0.3
stroke.Color = Color3.fromRGB(120, 80, 255)

-- Shadow effect
local shadow = Instance.new("Frame", main)
shadow.Name = "Shadow"
shadow.Size = UDim2.new(1, 6, 1, 6)
shadow.Position = UDim2.new(0, -3, 0, 3)
shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
shadow.BackgroundTransparency = 0.7
shadow.ZIndex = -1
local shadowCorner = Instance.new("UICorner", shadow)
shadowCorner.CornerRadius = UDim.new(0, 15)

-- Título com efeito
local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, 0, 0, 28)
title.Position = UDim2.new(0, 0, 0, 5)
title.BackgroundTransparency = 1
title.Text = "✨ @Nova.Br ✨"
title.TextColor3 = Color3.fromRGB(200, 180, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 18
title.TextStrokeTransparency = 0.8
title.TextStrokeColor3 = Color3.fromRGB(255, 255, 255)
title.ZIndex = 12

-- Botão principal melhorado
local btn = Instance.new("TextButton", main)
btn.Size = UDim2.new(1, -20, 0, 35)
btn.Position = UDim2.new(0, 10, 0, 38)
btn.BackgroundColor3 = Color3.fromRGB(80, 120, 85)
btn.BackgroundTransparency = 0.1
btn.Font = Enum.Font.GothamSemibold
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.TextSize = 16
btn.Text = "🚀 Auto Float v2 : OFF"
btn.ZIndex = 12
btn.TextStrokeTransparency = 0.9
btn.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)

-- Gradiente do botão
local btnGradient = Instance.new("UIGradient", btn)
btnGradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(60, 100, 65)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(100, 140, 105))
}
btnGradient.Rotation = 90

local btnCorner = Instance.new("UICorner", btn)
btnCorner.CornerRadius = UDim.new(0, 10)

-- Borda do botão
local btnStroke = Instance.new("UIStroke", btn)
btnStroke.Thickness = 1.5
btnStroke.Transparency = 0.5
btnStroke.Color = Color3.fromRGB(150, 255, 150)

-- Variáveis
local on = false
local platform = nil
local rainbow = 0
local platformParts = {}

-- FPS ESP Setup
local espFolder = Instance.new("Folder", workspace)
espFolder.Name = "NovaBrESP_Folder"

-- Função para criar plataforma melhorada
function createPlatform()
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return nil end
    
    if not platform then
        -- Plataforma principal
        platform = Instance.new("Part")
        platform.Size = Vector3.new(8, 0.8, 8)
        platform.Anchored = true
        platform.CanCollide = true
        platform.Transparency = 0.1
        platform.Material = Enum.Material.Neon
        platform.Name = "NovaBrPlatform"
        platform.Parent = workspace
        platform.Shape = Enum.PartType.Cylinder
        
        -- Efeito de partículas
        local attachment = Instance.new("Attachment", platform)
        local particles = Instance.new("ParticleEmitter", attachment)
        particles.Texture = "rbxasset://textures/particles/sparkles_main.dds"
        particles.Lifetime = NumberRange.new(1, 2)
        particles.Rate = 50
        particles.SpreadAngle = Vector2.new(45, 45)
        particles.Speed = NumberRange.new(2, 5)
        particles.Transparency = NumberSequence.new{
            NumberSequenceKeypoint.new(0, 0),
            NumberSequenceKeypoint.new(1, 1)
        }
        particles.Size = NumberSequence.new{
            NumberSequenceKeypoint.new(0, 0.1),
            NumberSequenceKeypoint.new(0.5, 0.3),
            NumberSequenceKeypoint.new(1, 0)
        }
        
        -- Bordas decorativas
        for i = 1, 8 do
            local border = Instance.new("Part")
            border.Size = Vector3.new(0.5, 1.5, 0.5)
            border.Anchored = true
            border.CanCollide = false
            border.Transparency = 0.2
            border.Material = Enum.Material.Neon
            border.Shape = Enum.PartType.Block
            border.Parent = workspace
            border.Name = "NovaBrPlatformBorder"
            table.insert(platformParts, border)
        end
        
        -- Efeito de onda
        local wave = Instance.new("Part")
        wave.Size = Vector3.new(10, 0.2, 10)
        wave.Anchored = true
        wave.CanCollide = false
        wave.Transparency = 0.7
        wave.Material = Enum.Material.Neon
        wave.Shape = Enum.PartType.Cylinder
        wave.Parent = workspace
        wave.Name = "NovaBrPlatformWave"
        table.insert(platformParts, wave)
    end
    return root
end

-- ESP melhorado
local espBoxes = {}

function addESP(plr)
    if plr == player then return end
    if espBoxes[plr] then
        if espBoxes[plr].Adornee and espBoxes[plr].Adornee.Parent then
            return
        end
    end
    
    local function makeBox()
        local char = plr.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        if root then
            local box = Instance.new("BoxHandleAdornment")
            box.Name = "NovaBrESP"
            box.Adornee = root
            box.AlwaysOnTop = true
            box.ZIndex = 10
            box.Size = Vector3.new(4.5, 7.5, 2.5)
            box.Transparency = 0.3
            box.Parent = espFolder
            espBoxes[plr] = box
        end
    end
    makeBox()
    plr.CharacterAdded:Connect(makeBox)
    plr.AncestryChanged:Connect(function()
        if not plr.Parent and espBoxes[plr] then
            espBoxes[plr]:Destroy()
            espBoxes[plr] = nil
        end
    end)
end

-- Remove ESP quando jogador sai
game.Players.PlayerRemoving:Connect(function(plr)
    if espBoxes[plr] then
        espBoxes[plr]:Destroy()
        espBoxes[plr] = nil
    end
end)

-- Inicializar ESP
for _, plr in ipairs(game.Players:GetPlayers()) do
    addESP(plr)
end
game.Players.PlayerAdded:Connect(addESP)

-- Loop principal com efeitos melhorados
game:GetService("RunService").RenderStepped:Connect(function()
    rainbow = (rainbow + 0.015) % 1
    
    -- Animações da UI
    local time = tick()
    stroke.Color = Color3.fromHSV((rainbow + 0.3) % 1, 0.8, 1)
    btnStroke.Color = Color3.fromHSV((rainbow + 0.6) % 1, 0.7, 1)
    title.TextColor3 = Color3.fromHSV((rainbow + 0.1) % 1, 0.5, 1)
    
    -- Efeito pulsante no título
    title.TextSize = 18 + math.sin(time * 3) * 1
    
    if on then
        local char = player.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        if root then
            if not platform or not platform.Parent then
                createPlatform()
            end
            
            -- Posição e cor da plataforma
            local pos = root.Position - Vector3.new(0, root.Size.Y/2 + 2, 0)
            platform.Position = pos
            platform.Color = Color3.fromHSV(rainbow, 0.8, 0.9)
            
            -- Animar bordas
            for i, border in ipairs(platformParts) do
                if border.Name == "NovaBrPlatformBorder" then
                    local angle = (i - 1) * (360 / 8) + time * 50
                    local x = pos.X + math.cos(math.rad(angle)) * 4
                    local z = pos.Z + math.sin(math.rad(angle)) * 4
                    border.Position = Vector3.new(x, pos.Y + 0.5, z)
                    border.Color = Color3.fromHSV((rainbow + i * 0.1) % 1, 0.9, 1)
                    border.Rotation = Vector3.new(0, angle, 0)
                elseif border.Name == "NovaBrPlatformWave" then
                    border.Position = Vector3.new(pos.X, pos.Y - 0.5, pos.Z)
                    border.Color = Color3.fromHSV((rainbow + 0.5) % 1, 0.6, 0.8)
                    border.Size = Vector3.new(10 + math.sin(time * 2) * 2, 0.2, 10 + math.sin(time * 2) * 2)
                end
            end
        else
            if platform then
                platform:Destroy()
                for _, part in ipairs(platformParts) do
                    if part then part:Destroy() end
                end
                platformParts = {}
                platform = nil
            end
        end
    else
        if platform then
            platform:Destroy()
            for _, part in ipairs(platformParts) do
                if part then part:Destroy() end
            end
            platformParts = {}
            platform = nil
        end
    end
    
    -- Atualizar ESP com cores dinâmicas
    for plr, box in pairs(espBoxes) do
        local char = plr.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        if root then
            box.Adornee = root
            box.Size = Vector3.new(4.5, 7.5, 2.5)
            box.Color3 = Color3.fromHSV((rainbow + (plr.UserId % 10) * 0.1) % 1, 0.9, 1)
            box.Visible = true
        else
            box.Visible = false
        end
    end
end)

-- Efeito de hover no botão
btn.MouseEnter:Connect(function()
    game:GetService("TweenService"):Create(btn, TweenInfo.new(0.2), {
        Size = UDim2.new(1, -15, 0, 38)
    }):Play()
end)

btn.MouseLeave:Connect(function()
    game:GetService("TweenService"):Create(btn, TweenInfo.new(0.2), {
        Size = UDim2.new(1, -20, 0, 35)
    }):Play()
end)

-- Clique do botão
btn.MouseButton1Click:Connect(function()
    on = not on
    btn.Text = "🚀 Auto Float v2 : " .. (on and "ON ⚡" or "OFF")
    
    -- Animação do botão
    local tween = game:GetService("TweenService"):Create(btn, TweenInfo.new(0.1), {
        Size = UDim2.new(1, -25, 0, 30)
    })
    tween:Play()
    tween.Completed:Connect(function()
        game:GetService("TweenService"):Create(btn, TweenInfo.new(0.1), {
            Size = UDim2.new(1, -20, 0, 35)
        }):Play()
    end)
    
    if on then
        createPlatform()
        btnGradient.Color = ColorSequence.new{
            ColorSequenceKeypoint.new(0, Color3.fromRGB(60, 150, 60)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(100, 200, 100))
        }
    else
        if platform then
            platform:Destroy()
            for _, part in ipairs(platformParts) do
                if part then part:Destroy() end
            end
            platformParts = {}
            platform = nil
        end
        btnGradient.Color = ColorSequence.new{
            ColorSequenceKeypoint.new(0, Color3.fromRGB(60, 100, 65)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(100, 140, 105))
        }
    end
end)

-- Recriar plataforma ao spawnar
player.CharacterAdded:Connect(function()
    if platform then 
        platform:Destroy() 
        for _, part in ipairs(platformParts) do
            if part then part:Destroy() end
        end
        platformParts = {}
        platform = nil 
    end
    if on then 
        wait(1.5) 
        createPlatform() 
    end
end)
