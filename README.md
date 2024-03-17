 --Vars
 LocalPlayer = game:GetService("Players").LocalPlayer
 Camera = workspace.CurrentCamera
 VirtualUser = game:GetService("VirtualUser")
 MarketplaceService = game:GetService("MarketplaceService")


 --Notification Handler
function SendNotification(Title, Message, Duration)
    game.StarterGui:SetCore("SendNotification", {
        Title = Title;
        Text = Message;
        Duration = Duration;
    })
end


 --Get Current Vehicle
 function GetCurrentVehicle()
     return LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") and LocalPlayer.Character.Humanoid.SeatPart and LocalPlayer.Character.Humanoid.SeatPart.Parent
 end
 
 --Regular TP
 function TP(cframe)
     GetCurrentVehicle():SetPrimaryPartCFrame(cframe)
 end
 
 --Velocity TP
 function VelocityTP(cframe)
     TeleportSpeed = math.random(600, 600)
     Car = GetCurrentVehicle()
     local BodyGyro = Instance.new("BodyGyro", Car.PrimaryPart)
     BodyGyro.P = 5000
     BodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
     BodyGyro.CFrame = Car.PrimaryPart.CFrame
     local BodyVelocity = Instance.new("BodyVelocity", Car.PrimaryPart)
     BodyVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)
     BodyVelocity.Velocity = CFrame.new(Car.PrimaryPart.Position, cframe.p).LookVector * TeleportSpeed
     wait((Car.PrimaryPart.Position - cframe.p).Magnitude / TeleportSpeed)
     BodyVelocity.Velocity = Vector3.new()
     wait(0.1)
     BodyVelocity:Destroy()
     BodyGyro:Destroy()
 end
 
 --Auto Farm
 StartPosition = CFrame.new(Vector3.new(338.369843, 14.3852158, 3146.229, 0.801708043, -7.18456405e-08, 0.597715855, 2.17021565e-08, 1, 9.10915219e-08, -0.597715855, -6.00570829e-08, 0.801708043), Vector3.new(-475.130127, 14.3862143, 2973.50757, 0.0697661191, -3.86354992e-09, 0.997563362, -6.94351729e-11, 1, 3.87784294e-09, -0.997563362, -3.39808043e-10, 0.0697661191))
EndPosition = CFrame.new(Vector3.new(-296.042053, 14.3862152, 3009.96973, 0.126643062, -4.45521131e-09, 0.991948366, -1.55558144e-08, 1, 6.47740128e-09, -0.991948366, -1.62508833e-08, 0.126643062), Vector3.new(-475.130127, 14.3862143, 2973.50757, 0.0697661191, -3.86354992e-09, 0.997563362, -6.94351729e-11, 1, 3.87784294e-09, -0.997563362, -3.39808043e-10, 0.0697661191))
AutoFarmFunc = coroutine.create(function()
    while wait() do
        if not AutoFarm then
            AutoFarmRunning = false
            coroutine.yield()
        end
        AutoFarmRunning = true
        pcall(function()
            if not GetCurrentVehicle() and tick() - (LastNotif or 0) > 5 then
                LastNotif = tick()
                SendNotification("wtf Hub", "มึงขึ้นรถก่อน sus")
            else
                TP(StartPosition + (TouchTheRoad and Vector3.new() or Vector3.new(0, 1, 0)))
                VelocityTP(EndPosition + (TouchTheRoad and Vector3.new() or Vector3.new(0, 1, 0)))
                TP(EndPosition + (TouchTheRoad and Vector3.new() or Vector3.new(0, 1, 0)))
                VelocityTP(StartPosition + (TouchTheRoad and Vector3.new() or Vector3.new(0, 1, 0)))
            end
        end)
    end
end)
 
 --Anti AFK
 AntiAFK = true
 LocalPlayer.Idled:Connect(function()
     VirtualUser:CaptureController()
     VirtualUser:ClickButton2(Vector2.new(), Camera.CFrame)
 end)
 
 --UI
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/FujiXDDDDDDDDD1/GUI/main/README.md"))()
local venyx = library.new("wtf Hub", 5013109572)

--Themes
local themes = {
    Background = Color3.fromRGB(23, 23, 23),
    Glow = Color3.fromRGB(46, 40, 172),
    Accent = Color3.fromRGB(30, 20, 53),
    LightContrast = Color3.fromRGB(20, 20, 20),
    DarkContrast = Color3.fromRGB(21, 18, 27),
    TextColor = Color3.fromRGB(71, 104, 184)
}

--Pages
local page1 = venyx:addPage("Main")
local page2 = venyx:addPage("Other")

--Page 1
local FirstSection1 = page1:addSection("Auto Farm")

FirstSection1:addToggle(
    "Start",
    nil,
    function(value)
        AutoFarm = value
        if value and not AutoFarmRunning then
            coroutine.resume(AutoFarmFunc)
        end
    end
)
--Page 2

    "Anti AFK",
    true,
    function(value)
        AntiAFK = value
    end
)
SecondSection2:addKeybind(
    "Toggle Keybind",
    Enum.KeyCode.RightShift,
    function()
        venyx:toggle()
    end,
    function(key)
        Keybind = key.KeyCode.Name
    end
)
for theme, color in pairs(themes) do
    SecondSection2:addColorPicker(
        theme,
        color,
        function(color3)
            venyx:setTheme(theme, color3)
        end
    )
end


