 

local ScreenGui = Instance.new("ScreenGui")

local OpenFrame = Instance.new("Frame")

local Open = Instance.new("TextButton")

local Main = Instance.new("Frame")

local header = Instance.new("Frame")

local Name = Instance.new("TextLabel")

local Close = Instance.new("TextButton")

local Credits = Instance.new("TextLabel")

local ESP = Instance.new("TextButton")

local SilentAim = Instance.new("TextButton")

local PHGUI = Instance.new("TextButton")

 

--Properties:

 

ScreenGui.Parent = game.CoreGui

ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

 

OpenFrame.Name = "OpenFrame"

OpenFrame.Parent = ScreenGui

OpenFrame.Active = true

OpenFrame.BackgroundColor3 = Color3.fromRGB(0, 255, 0)

OpenFrame.Position = UDim2.new(0, 0, 0.443877548, 0)

OpenFrame.Size = UDim2.new(0, 143, 0, 37)

 

Open.Name = "Open"

Open.Parent = OpenFrame

Open.BackgroundColor3 = Color3.fromRGB(0, 255, 127)

Open.Size = UDim2.new(0, 143, 0, 37)

Open.Font = Enum.Font.Cartoon

Open.Text = "Open"

Open.TextColor3 = Color3.fromRGB(0, 0, 0)

Open.TextScaled = true

Open.TextSize = 30.000

Open.TextWrapped = true

Open.MouseButton1Down:connect(function()

Main.Visible = true

OpenFrame.Visible = false

end)

 

Main.Name = "Main"

Main.Parent = ScreenGui

Main.Active = true

Main.BackgroundColor3 = Color3.fromRGB(0, 0, 0)

Main.BackgroundTransparency = 0.500

Main.Position = UDim2.new(-0.00216758158, 0, 0.505102038, 0)

Main.Size = UDim2.new(0, 145, 0, 265)

Main.Visible = false

Main.Draggable = true

 

header.Name = "header"

header.Parent = Main

header.BackgroundColor3 = Color3.fromRGB(29, 29, 29)

header.Position = UDim2.new(-0.00254832301, 0, -0.00186459522, 0)

header.Size = UDim2.new(0, 145, 0, 29)

 

Name.Name = "Name"

Name.Parent = header

Name.BackgroundColor3 = Color3.fromRGB(0, 255, 255)

Name.BackgroundTransparency = 1.000

Name.Position = UDim2.new(0.0758621693, 0, 0.0689654946, 0)

Name.Size = UDim2.new(0, 122, 0, 27)

Name.Font = Enum.Font.SourceSans

Name.Text = "Phantom Hub"

Name.TextColor3 = Color3.fromRGB(0, 255, 255)

Name.TextSize = 14.000

 

Close.Name = "Close"

Close.Parent = header

Close.BackgroundColor3 = Color3.fromRGB(255, 255, 255)

Close.BackgroundTransparency = 1.000

Close.Position = UDim2.new(0.841379464, 0, 0.068965517, 0)

Close.Size = UDim2.new(0, 23, 0, 26)

Close.Font = Enum.Font.SourceSans

Close.Text = "X"

Close.TextColor3 = Color3.fromRGB(0, 255, 255)

Close.TextSize = 50.000

Close.MouseButton1Down:connect(function()

OpenFrame.Visible = true

Main.Visible = false

end)

 

Credits.Name = "Credits"

Credits.Parent = header

Credits.BackgroundColor3 = Color3.fromRGB(36, 36, 36)

Credits.BorderColor3 = Color3.fromRGB(0, 255, 255)

Credits.Position = UDim2.new(0.0068962574, 0, 9.13793087, 0)

Credits.Size = UDim2.new(0, 167, 0, 22)

Credits.Font = Enum.Font.SourceSans

Credits.Text = "- i didnt write any of the scripts"

Credits.TextColor3 = Color3.fromRGB(0, 255, 255)

Credits.TextSize = 15.000

 

ESP.Name = "ESP"

ESP.Parent = Main

ESP.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

ESP.Position = UDim2.new(-0.0275861546, 0, 0.105660379, 0)

ESP.Size = UDim2.new(0, 147, 0, 25)

ESP.Font = Enum.Font.SourceSans

ESP.Text = "ESP"

ESP.TextColor3 = Color3.fromRGB(0, 255, 255)

ESP.TextSize = 14.000

ESP.MouseButton1Down:connect(function()

local funcs = {

    getgc = getgc or get_gc_objects,

}

 

local Services = setmetatable({

    LocalPlayer = game:GetService("Players").LocalPlayer,

    Mouse = game:GetService("Players").LocalPlayer:GetMouse(),

    Camera = workspace.CurrentCamera,

}, {

    __index = function(self, idx)

        if rawget(self, idx) then

            return rawget(self, idx)

        else

            return game:GetService(idx)

        end

    end

})

 

local Settings = {

    Enabled = true,

    Color3 = Color3.new(1, 1, 1),

    Transparency = 0,

    AlwaysOnTop = true,

}

 

function funcs:GetHiddenFunction(name)

    for i, v in pairs(funcs.getgc()) do

        if type(v) == "function" and getinfo(v).name == name then

           return v

        end

    end

end

 

local Characters = debug.getupvalues(funcs:GetHiddenFunction("getbodyparts"))[1]

 

if Characters then

    function funcs:IsSuitable(player)

        if player and player.Team ~= Services.LocalPlayer.Team and Characters[player] and Characters[player].char and Characters[player].head then

            return true

        end

    end

 

    function funcs:CreateChams(player)

        Services.RunService.RenderStepped:Connect(function()

            if funcs:IsSuitable(player) and Settings.Enabled then

                for i, v in pairs(Characters[player]) do

                    if v:IsA("BasePart") and not v:FindFirstChild("BoxHandleAdornment") and v.Name ~= "HumanoidRootPart" then

                        local Cham = Instance.new("BoxHandleAdornment", v)

 

                        for k, x in pairs(Settings) do

                            if k ~= "Enabled" then

                                Cham[k] = x

                            end

                        end

 

                        Cham.ZIndex = 5

                        Cham.Adornee = v

                        Cham.Size = v.Size

                    elseif v:FindFirstChild("BoxHandleAdornment") then

                        for k, x in pairs(Settings) do

                            if k ~= "Enabled" then

                                v:FindFirstChild("BoxHandleAdornment")[k] = x

                            end

                        end

                    end

                end

            end

        end)

    end

 

    for i, v in pairs(Services.Players:GetPlayers()) do

        funcs:CreateChams(v)

    end

 

    Services.Players.PlayerAdded:Connect(function(v)

        funcs:CreateChams(v)

    end)

else

    error("game updated srry Undecided check on my v3rm for a update")

end

 

end)

 

SilentAim.Name = "SilentAim"

SilentAim.Parent = Main

SilentAim.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

SilentAim.BackgroundTransparency = 0.500

SilentAim.Position = UDim2.new(-0.0137930512, 0, 0.381132066, 0)

SilentAim.Size = UDim2.new(0, 147, 0, 25)

SilentAim.Font = Enum.Font.SourceSans

SilentAim.Text = "Silent Aim"

SilentAim.TextColor3 = Color3.fromRGB(0, 255, 255)

SilentAim.TextSize = 14.000

SilentAim.MouseButton1Down:connect(function()

loadstring(game:HttpGet("https://pastebin.com/raw/1McuqKm7"))()

end)

 

PHGUI.Name = "PH GUI"

PHGUI.Parent = Main

PHGUI.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

PHGUI.BackgroundTransparency = 0.500

PHGUI.Position = UDim2.new(-0.0206896029, 0, 0.241509438, 0)

PHGUI.Size = UDim2.new(0, 147, 0, 25)

PHGUI.Font = Enum.Font.SourceSans

PHGUI.Text = "Phantom Forces GUI"

PHGUI.TextColor3 = Color3.fromRGB(0, 255, 255)

PHGUI.TextSize = 14.000

PHGUI.MouseButton1Down:connect(function()

loadstring(game:HttpGet(('https://pastebin.com/raw/8KHFpex7'),true))()

end)
