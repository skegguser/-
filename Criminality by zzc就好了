	--[[
		TODO
		--
		rewrite the entire thing
		ESP
		{
			entity
			arrows
			flags --// desync, cheat, mod, manipulated, target
		}
		MISC
		{
			Mod Detector
			Cheat Detector
			uh all misc from v1 and v0 and old vers xddd
		}
	]]
	--// setup / globals table

	if not LPH_OBFUSCATED then
		LPH_JIT = function(Function)
			return Function
		end
		LPH_JIT_MAX = function(Function)
			return Function
		end
		LPH_NO_VIRTUALIZE = function(Function)
			return Function
		end
		LPH_NO_UPVALUES = function(Function)
			return function(...)
				return Function(...)
			end
		end
		LPH_ENCSTR = function(String)
			return String
		end
		LPH_ENCNUM = function(Number)
			return Number
		end
		LPH_CRASH = function()
			return print("crash >:(")
		end
		LRM_SEND_WEBHOOK = function(...)
			return
		end
		LRM_LinkedDiscordID = "Oxycontin"
		LRM_Uid = "19"
		LRM_SecondsLeft = "86400"
	end
	--
	Globals = {}
	Globals.Services = {}
	local Cache = {}
	Globals.Services = setmetatable({}, {
		__index = function(_, Service)
			if not Cache[Service] then
				Cache[Service] = cloneref(game:GetService(Service))
			end
			--
			return Cache[Service]
		end,
	})
	Globals.DeepCopy = LPH_NO_VIRTUALIZE(function(Table)
		local Copy = {}
		for k, v in pairs(Table) do
			if type(v) == "table" then
				Copy[k] = Globals.DeepCopy(v)
			else
				Copy[k] = v
			end
		end
		return Copy
	end)
	Globals.Listeners = {}
	Globals.Cache = {}
	Globals.Client = Globals.Services.Players.LocalPlayer
	Globals.ClientCharacter = Globals.Client.Character
	Globals.CoreGui = Globals.Services.CoreGui
	Globals.Humanoid = Globals.ClientCharacter:FindFirstChildWhichIsA("Humanoid")
	--
	Globals.Camera = workspace.CurrentCamera
	--
	Globals.Assets = {
		Hitsounds = {
			Neverlose = "rbxassetid://8726881116",
			Gamesense = "rbxassetid://4817809188",
			One = "rbxassetid://7380502345",
			Bell = "rbxassetid://6534947240",
			Rust = "rbxassetid://1255040462",
			TF2 = "rbxassetid://2868331684",
			Slime = "rbxassetid://6916371803",
			["Among Us"] = "rbxassetid://5700183626",
			Minecraft = "rbxassetid://4018616850",
			["CS:GO"] = "rbxassetid://6937353691",
			Saber = "rbxassetid://8415678813",
			Baimware = "rbxassetid://3124331820",
			Osu = "rbxassetid://7149255551",
			["TF2 Critical"] = "rbxassetid://296102734",
			Bat = "rbxassetid://3333907347",
			["Call of Duty"] = "rbxassetid://5952120301",
			Bubble = "rbxassetid://6534947588",
			Pick = "rbxassetid://1347140027",
			Pop = "rbxassetid://198598793",
			Bruh = "rbxassetid://4275842574",
			Bamboo = "rbxassetid://3769434519",
			Crowbar = "rbxassetid://546410481",
			Weeb = "rbxassetid://6442965016",
			Beep = "rbxassetid://8177256015",
			Bambi = "rbxassetid://8437203821",
			Stone = "rbxassetid://3581383408",
			["Old Fatality"] = "rbxassetid://6607142036",
			Click = "rbxassetid://8053704437",
			Ding = "rbxassetid://7149516994",
			Snow = "rbxassetid://6455527632",
			Laser = "rbxassetid://7837461331",
			Mario = "rbxassetid://2815207981",
			Steve = "rbxassetid://4965083997",
			Snowdrake = "rbxassetid://7834724809",
		},
		Textures = {
			["Web"] = "rbxassetid://301464986",
			["Swirl"] = "rbxassetid://8133639623",
			["Checkers"] = "rbxassetid://5790215150",
			["CandyCane"] = "rbxassetid://6853532738",
			["Dots"] = "rbxassetid://5830615971",
			["Scanning"] = "rbxassetid://5843010904",
			["Bubbles"] = "rbxassetid://1461576423",
			["Player FF Texture"] = "rbxassetid://4494641460",
			["Shield Forcefield"] = "rbxassetid://361073795",
			["Water"] = "rbxasset://textures/water/normal_21.dds",
			["America"] = "rbxassetid://936775406",
			["Rainbow"] = "rbxassetid://252684207",
			["Akatsuki"] = "rbxassetid://10913193650",
			["None"] = "",
		},
		Shaders = {
			["Galaxy"] = "rbxassetid://13726625670",
			["Universe"] = "rbxassetid://16666870788",
			["Groovy"] = "rbxassetid://17672592862",
			["Liquid"] = "rbxassetid://17253872107",
			["Israel"] = "rbxassetid://964998527",
			["Troll"] = "rbxassetid://17673708830",
			["Russia"] = "rbxassetid://12257572118",
			["Belarus"] = "rbxassetid://13567566609",
			["Zelensky"] = "rbxassetid://14671159559",
		},
		ShaderAnimatedTextures = {},
	}
	-- funcs
	Globals.Functions = {}
	function Globals.Functions:closestpart()
		local lp = Globals.Services.Players.LocalPlayer
		local cam = Globals.Services.Workspace.CurrentCamera
		local mouse = lp:GetMouse()
		local closest, headpartshi = math.huge, nil
		for _, plr in ipairs(Globals.Services.Players:GetPlayers()) do
			if plr ~= lp and plr.Character then
				local head = plr.Character:FindFirstChild("Head")
				if head then
					local screenpos, onscreen = cam:WorldToScreenPoint(head.Position)
					if onscreen then
						local dist = (Vector2.new(screenpos.X, screenpos.Y) - Vector2.new(mouse.X, mouse.Y)).Magnitude
						if dist < closest then
							closest = dist
							headpartshi = head
						end
					end
				end
			end
		end
		return headpartshi
	end
	--// Engine setup
	Globals.Engine = {}
	--debug("Global table loaded")
	local Engine = Globals.Engine

	local RunService = game:GetService("RunService")

	local function Safe(f, ...)
		local ok, err = pcall(f, ...)
		if not ok then
			warn("[Engine Error]:", err)
		end
	end

	function Engine:Thread(f, ...)
		task.spawn(Safe, f, ...)
	end

	function Engine:Delay(t, f, ...)
		task.delay(t, Safe, f, ...)
	end

	function Engine:Connect(signal, f)
		return signal:Connect(function(...)
			Safe(f, ...)
		end)
	end

	function Engine:Heartbeat(f)
		return RunService.Heartbeat:Connect(function(dt)
			Safe(f, dt)
		end)
	end

	function Engine:Stepped(f)
		return RunService.Stepped:Connect(function(t, dt)
			Safe(f, t, dt)
		end)
	end

	function Engine:Render(f)
		return RunService.RenderStepped:Connect(function(dt)
			Safe(f, dt)
		end)
	end

	Engine.Queue = {}

	function Engine:QueueTask(f)
		table.insert(self.Queue, f)
	end

	Engine.QueueConnection = RunService.Heartbeat:Connect(function()
		local tasks = Engine.Queue
		if #tasks > 0 then
			local f = table.remove(tasks, 1)
			Safe(f)
		end
	end)
	-- misc globals
	Globals.rgb = Color3.fromRGB
	State = {
		Manipulated = false,
		ManipulatedIndex = 0,
		IsShooting = false,
		CurrentData = nil,
		FakePart = nil,
		ManipulatedPosition = nil,
		ManipulatedDirection = nil,
		TargetPart = nil,
	}
	Manipulation = {
		Bind = false,
		Enabled = true,
		Distance = 30,
		Visualize = true,
		VisualizeColor = Color3.fromRGB(255, 50, 50),
	}

	Current = CFrame.new()

	Fake = {
		Real = {
			CFrame = CFrame.new(),
		},
		Fake = {
			CFrame = CFrame.new(),
		},
	}
	--// client ban bypass
	eventbypass = hookmetamethod(game, "__namecall", function(self, ...)
		local method = getnamecallmethod()
		local args = { ... }
		if not checkcaller() and self.Name == "ProjectileInflict" and method == "FireServer" then
			local isTargetArgs = false
			if typeof(args[3]) == "number" and args[3] >= 0 and args[3] <= 10 then
				warn("ban blocked")
				return coroutine.yield()
			end
		end
		return eventbypass(self, ...)
	end)
	--
	print = function(str)
		--rconsoleprint(tostring(str) .. "\n")
		print(tostring(str))
	end
	FromRGB = Color3.fromRGB
	hex = Color3.fromHex
	do --
		LPH_JIT_MAX(function() --// UI Setup
			local GameMT = getrawmetatable(game)
			local GameIndex = GameMT.__index
			local GetServiceFunc = GameIndex(game, "GetService")
			local HttpGet = GameIndex(game, "HttpGet")
			local GetService = function(...)
				return cloneref(GetServiceFunc(game, ...))
			end
			--
			local FindFirstChild = GameIndex(game, "FindFirstChild")
			local FindFirstChildOfClass = GameIndex(game, "FindFirstChildOfClass")
			FindFirstAncestor = GameIndex(game, "FindFirstAncestor")
			local FindFirstAncestorOfClass = GameIndex(game, "FindFirstAncestorOfClass")
			local FindFirstChildWhichIsA = GameIndex(game, "FindFirstChildWhichIsA")
			GetDescendants = GameIndex(game, "GetDescendants")
			local IsDescendantOf = GameIndex(game, "IsDescendantOf")
			local IsAncestorOf = GameIndex(game, "IsAncestorOf")
			local WaitForChild = GameIndex(game, "WaitForChild")
			local IsA = GameIndex(game, "IsA")
			local IsDescendantOf = GameIndex(game, "IsDescendantOf")
			local IsAncestorOf = GameIndex(game, "IsAncestorOf")
			local GetChildren = GameIndex(game, "GetChildren")
			local WaitForChild = GameIndex(game, "WaitForChild")

			-- Services
			local Cache = {}
			Services = setmetatable({}, {
				__index = function(_, Service)
					if not Cache[Service] then
						Cache[Service] = GetService(Service)
					end
					--
					return Cache[Service]
				end,
			})

			-- Camera
			local CurrentCamera = Services.Workspace.CurrentCamera
			local CameraMT = getrawmetatable(CurrentCamera)
			local CameraIndex = CameraMT.__index
			--
			local ScreenSize = CameraIndex(CurrentCamera, "ViewportSize")
			local WorldToViewportPoint = CameraIndex(CurrentCamera, "WorldToViewportPoint")
			local WorldToScreenPoint = CameraIndex(CurrentCamera, "WorldToScreenPoint")
			local ViewportPointToRay = CameraIndex(CurrentCamera, "ViewportPointToRay")

			-- Players
			local PlayersMT = getrawmetatable(Services.Players)
			local PlayersIndex = PlayersMT.__index
			--
			local Client = PlayersIndex(Services.Players, "LocalPlayer")
			local GetPlayers = PlayersIndex(Services.Players, "GetPlayers")

			-- Tables
			Utils, Menu, Flags, Character, Visuals2, Env =
				{
					Connections = {},
					Screens = {},
					Instances = {},
					Math = {},
					Table = {},
					String = {},
					Configs = { Local = {} },
					Font = { Count = 0, Fonts = {}, Enums = {}, String = {} },
					Combat = {},
					Hooks = {
						Metamethods = {},
						Spoofed = {},
						Detours = {},
					},
					Funcs = {},
					ShaderAnimatedTextures = {},
				}, {
					FileName = "Niggahack",
					PendingToasts = {},
					ActiveToasts = {},
					Storage = {},
					Objects = {},
					Themes = {},
					Labels = {},
					--
					Pages = {},
					--
					Sections = {
						Addons = {},
					},
					--
					Presets = {
						Themes = {
							Abyss = {
								["Accent"] = Color3.fromRGB(140, 135, 180),
								["Arrow Color"] = Color3.fromRGB(255, 255, 255),
								["Lighter Background"] = Color3.fromRGB(30, 30, 30),
								["Light Background"] = Color3.fromRGB(25, 25, 25),
								["Dark Background"] = Color3.fromRGB(20, 20, 20),
								["Outline"] = Color3.fromRGB(10, 10, 10),
								["Lighter Inline"] = Color3.fromRGB(50, 50, 50),
								["Light Inline"] = Color3.fromRGB(45, 45, 45),
								["Dark Inline"] = Color3.fromRGB(30, 30, 30),
								["Light Text"] = Color3.fromRGB(255, 255, 255),
								["Dark Text"] = Color3.fromRGB(175, 175, 175),
							},
							RosePine = {
								["Accent"] = Color3.fromRGB(218, 184, 255),
								["Arrow Color"] = Color3.fromRGB(255, 214, 165),
								["Lighter Background"] = Color3.fromRGB(38, 42, 59),
								["Light Background"] = Color3.fromRGB(30, 34, 46),
								["Dark Background"] = Color3.fromRGB(24, 28, 39),
								["Outline"] = Color3.fromRGB(64, 72, 93),
								["Lighter Inline"] = Color3.fromRGB(47, 51, 69),
								["Light Inline"] = Color3.fromRGB(55, 60, 80),
								["Dark Inline"] = Color3.fromRGB(44, 48, 63),
								["Light Text"] = Color3.fromRGB(252, 220, 236),
								["Dark Text"] = Color3.fromRGB(162, 172, 189),
							},
							Fatality = {
								["Accent"] = Color3.fromRGB(240, 15, 80),
								["Arrow Color"] = Color3.fromRGB(200, 200, 255),
								["Lighter Background"] = Color3.fromRGB(35, 25, 70),
								["Light Background"] = Color3.fromRGB(30, 20, 65),
								["Dark Background"] = Color3.fromRGB(25, 20, 50),
								["Outline"] = Color3.fromRGB(15, 15, 40),
								["Lighter Inline"] = Color3.fromRGB(55, 45, 85),
								["Light Inline"] = Color3.fromRGB(50, 40, 80),
								["Dark Inline"] = Color3.fromRGB(35, 25, 65),
								["Light Text"] = Color3.fromRGB(200, 200, 255),
								["Dark Text"] = Color3.fromRGB(175, 175, 175),
							},
							Neverlose = {
								["Accent"] = Color3.fromRGB(0, 180, 240),
								["Arrow Color"] = Color3.fromRGB(255, 255, 255),
								["Lighter Background"] = Color3.fromRGB(0, 15, 30),
								["Light Background"] = Color3.fromRGB(0, 10, 25),
								["Dark Background"] = Color3.fromRGB(5, 5, 20),
								["Outline"] = Color3.fromRGB(0, 0, 5),
								["Lighter Inline"] = Color3.fromRGB(15, 35, 45),
								["Light Inline"] = Color3.fromRGB(10, 30, 40),
								["Dark Inline"] = Color3.fromRGB(5, 20, 30),
								["Light Text"] = Color3.fromRGB(255, 255, 255),
								["Dark Text"] = Color3.fromRGB(175, 175, 175),
							},
							Aimware = {
								["Accent"] = Color3.fromRGB(200, 40, 40),
								["Arrow Color"] = Color3.fromRGB(232, 232, 232),
								["Lighter Background"] = Color3.fromRGB(43, 43, 43),
								["Light Background"] = Color3.fromRGB(35, 35, 35),
								["Dark Background"] = Color3.fromRGB(25, 25, 25),
								["Outline"] = Color3.fromRGB(0, 0, 5),
								["Lighter Inline"] = Color3.fromRGB(60, 60, 60),
								["Light Inline"] = Color3.fromRGB(55, 55, 55),
								["Dark Inline"] = Color3.fromRGB(35, 35, 35),
								["Light Text"] = Color3.fromRGB(232, 232, 232),
								["Dark Text"] = Color3.fromRGB(175, 175, 175),
							},
							Onetap = {
								["Accent"] = Color3.fromRGB(221, 168, 93),
								["Arrow Color"] = Color3.fromRGB(214, 217, 224),
								["Lighter Background"] = Color3.fromRGB(44, 48, 55),
								["Light Background"] = Color3.fromRGB(35, 40, 45),
								["Dark Background"] = Color3.fromRGB(31, 33, 37),
								["Outline"] = Color3.fromRGB(0, 0, 0),
								["Lighter Inline"] = Color3.fromRGB(83, 86, 93),
								["Light Inline"] = Color3.fromRGB(78, 81, 88),
								["Dark Inline"] = Color3.fromRGB(65, 68, 75),
								["Light Text"] = Color3.fromRGB(214, 217, 224),
								["Dark Text"] = Color3.fromRGB(175, 175, 175),
							},
							Entropy = {
								["Accent"] = Color3.fromRGB(129, 187, 233),
								["Arrow Color"] = Color3.fromRGB(220, 220, 220),
								["Lighter Background"] = Color3.fromRGB(61, 58, 67),
								["Light Background"] = Color3.fromRGB(55, 52, 61),
								["Dark Background"] = Color3.fromRGB(48, 47, 55),
								["Outline"] = Color3.fromRGB(10, 10, 10),
								["Lighter Inline"] = Color3.fromRGB(81, 78, 87),
								["Light Inline"] = Color3.fromRGB(76, 74, 82),
								["Dark Inline"] = Color3.fromRGB(65, 63, 71),
								["Light Text"] = Color3.fromRGB(220, 220, 220),
								["Dark Text"] = Color3.fromRGB(175, 175, 175),
							},
							Interwebz = {
								["Accent"] = Color3.fromRGB(201, 101, 75),
								["Arrow Color"] = Color3.fromRGB(252, 252, 252),
								["Lighter Background"] = Color3.fromRGB(41, 31, 56),
								["Light Background"] = Color3.fromRGB(35, 25, 50),
								["Dark Background"] = Color3.fromRGB(31, 22, 43),
								["Outline"] = Color3.fromRGB(26, 26, 26),
								["Lighter Inline"] = Color3.fromRGB(69, 59, 84),
								["Light Inline"] = Color3.fromRGB(64, 54, 79),
								["Dark Inline"] = Color3.fromRGB(55, 45, 70),
								["Light Text"] = Color3.fromRGB(252, 252, 252),
								["Dark Text"] = Color3.fromRGB(168, 168, 168),
							},
							Dracula = {
								["Accent"] = Color3.fromRGB(154, 129, 179),
								["Arrow Color"] = Color3.fromRGB(180, 180, 184),
								["Lighter Background"] = Color3.fromRGB(42, 44, 56),
								["Light Background"] = Color3.fromRGB(37, 39, 51),
								["Dark Background"] = Color3.fromRGB(37, 39, 48),
								["Outline"] = Color3.fromRGB(32, 33, 38),
								["Lighter Inline"] = Color3.fromRGB(65, 61, 82),
								["Light Inline"] = Color3.fromRGB(60, 56, 77),
								["Dark Inline"] = Color3.fromRGB(51, 47, 68),
								["Light Text"] = Color3.fromRGB(180, 180, 184),
								["Dark Text"] = Color3.fromRGB(136, 136, 139),
							},
							Vape = {
								["Accent"] = Color3.fromRGB(38, 134, 106),
								["Arrow Color"] = Color3.fromRGB(220, 220, 220),
								["Lighter Background"] = Color3.fromRGB(31, 31, 31),
								["Light Background"] = Color3.fromRGB(26, 26, 26),
								["Dark Background"] = Color3.fromRGB(26, 26, 26),
								["Outline"] = Color3.fromRGB(10, 10, 10),
								["Lighter Inline"] = Color3.fromRGB(59, 59, 59),
								["Light Inline"] = Color3.fromRGB(54, 54, 54),
								["Dark Inline"] = Color3.fromRGB(45, 45, 45),
								["Light Text"] = Color3.fromRGB(220, 220, 220),
								["Dark Text"] = Color3.fromRGB(175, 175, 175),
							},
							Niggahack = {
								["Accent"] = Color3.fromRGB(255, 41, 123),
								["Arrow Color"] = Color3.fromRGB(255, 41, 123),
								["Lighter Background"] = Color3.fromRGB(25, 20, 25),
								["Light Background"] = Color3.fromRGB(20, 15, 20),
								["Dark Background"] = Color3.fromRGB(15, 10, 15),
								["Outline"] = Color3.fromRGB(10, 5, 10),
								["Lighter Inline"] = Color3.fromRGB(40, 35, 40),
								["Light Inline"] = Color3.fromRGB(35, 30, 35),
								["Dark Inline"] = Color3.fromRGB(25, 20, 25),
								["Light Text"] = Color3.fromRGB(255, 255, 255),
								["Dark Text"] = Color3.fromRGB(180, 180, 180),
							},
						},
					},
					--
					Inputs = {
						KeyCodes = {
							["Q"] = true,
							["W"] = true,
							["E"] = true,
							["R"] = true,
							["T"] = true,
							["Y"] = true,
							["U"] = true,
							["I"] = true,
							["O"] = true,
							["P"] = true,
							["A"] = true,
							["S"] = true,
							["D"] = true,
							["F"] = true,
							["G"] = true,
							["H"] = true,
							["J"] = true,
							["K"] = true,
							["L"] = true,
							["Z"] = true,
							["X"] = true,
							["C"] = true,
							["V"] = true,
							["B"] = true,
							["N"] = true,
							["M"] = true,
							["Hash"] = true,
							["One"] = true,
							["Two"] = true,
							["Three"] = true,
							["Four"] = true,
							["Five"] = true,
							["Six"] = true,
							["Seveen"] = true,
							["Eight"] = true,
							["Nine"] = true,
							["Zero"] = true,
							["Minus"] = true,
							["Equals"] = true,
							["F1"] = true,
							["F2"] = true,
							["F3"] = true,
							["F4"] = true,
							["F5"] = true,
							["F6"] = true,
							["F7"] = true,
							["F8"] = true,
							["F9"] = true,
							["F10"] = true,
							["F11"] = true,
							["F12"] = true,
							["Insert"] = true,
							["Tab"] = true,
							["Home"] = true,
							["End"] = true,
							["LeftAlt"] = true,
							["LeftControl"] = true,
							["LeftShift"] = true,
							["RightAlt"] = true,
							["RightControl"] = true,
							["RightShift"] = true,
							["CapsLock"] = true,
							["BackSlash"] = true,
							["Space"] = true,
							["Up"] = true,
							["Left"] = true,
							["Down"] = true,
							["Right"] = true,
							["MouseButton1"] = true,
							["MouseButton2"] = true,
							["MouseButton3"] = true,
							["ButtonL2"] = true,
							["ButtonR2"] = true,
							["ButtonL1"] = true,
							["ButtonR1"] = true,
							["ButtonStart"] = true,
							["ButtonSelect"] = true,
							["ButtonY"] = true,
							["ButtonX"] = true,
							["ButtonA"] = true,
							["ButtonB"] = true,
							["ButtonL3"] = true,
							["ButtonR3"] = true,
							["DPadUp"] = true,
							["DPadLeft"] = true,
							["DPadDown"] = true,
							["DPadRight"] = true,
						},
						Shortened = {
							["BackSlash"] = "Bs",
							["Hash"] = "Hs",
							["End"] = "En",
							["One"] = "1",
							["Two"] = "2",
							["Three"] = "3",
							["Four"] = "4",
							["Five"] = "5",
							["Six"] = "6",
							["Seven"] = "7",
							["Eight"] = "8",
							["Nine"] = "9",
							["Zero"] = "0",
							["Minus"] = "Mn",
							["Equals"] = "Eq",
							["Home"] = "Hm",
							["Space"] = "Sp",
							["Insert"] = "In",
							["LeftAlt"] = "La",
							["LeftControl"] = "Lc",
							["LeftShift"] = "Ls",
							["RightAlt"] = "Ra",
							["RightControl"] = "Rc",
							["RightShift"] = "Rs",
							["CapsLock"] = "Ca",
							["Left"] = "Lf",
							["Down"] = "Dw",
							["Right"] = "Rg",
							["MouseButton1"] = "M1",
							["MouseButton2"] = "M2",
							["MouseButton3"] = "M3",
							["ButtonL2"] = "L2",
							["ButtonR2"] = "R2",
							["ButonL1"] = "L1",
							["ButtonR1"] = "R1",
							["ButtonStart"] = "St",
							["ButtonSelect"] = "Sl",
							["ButtonY"] = "Cy",
							["ButtonX"] = "Cx",
							["ButtonA"] = "Ca",
							["ButtonB"] = "Cb",
							["ButtonL3"] = "L3",
							["ButtonR3"] = "R3",
							["DPadUp"] = "Up",
							["DPadLeft"] = "Lf",
							["DPadDown"] = "Dw",
							["DPadRight"] = "Rg",
						},
						Modes = {
							"On Hold",
							"Off Hold",
							"On Toggle",
							"Off Toggle",
							"Always",
						},
					},
					--
					Images = {
						Saturation = "rbxassetid://14639149555",
						Hue = "rbxassetid://14639140994",
						Alpha = "rbxassetid://14639150937",
					},
				}, {}, {}, { Fonts = {
					["SmallestPixel"] = { nil, 9 },
					["Montserrat"] = { nil, 13 },
					["Minecraftia"] = { nil, 10 },
				} }, {
					Unloaded = false,
					Invite = "discord.gg/PST3seJr",
					Target = {},
					Loaded = false,
				}

			-- Functions
			do
				do -- Folders
					if not isfolder(Menu.FileName) then
						makefolder(Menu.FileName)
						makefolder(Menu.FileName .. "/Configs")
						--makefolder(`{Menu.FileName}/Configs`)
					end
				end
				--
				do -- Utils
					LPH_JIT_MAX(function()
						function Utils:Create(Type, Properties, Ignore)
							local Self = self
							local Object = Instance.new(Type)
							--
							if Type == "ScreenGui" and Ignore then
								Self.Screens[Object] = true
							end
							--
							Self:Update(Object, Properties)
							--
							Self.Instances[Object] = true
							--
							return Object
						end
						--
						function Utils:Update(Object, Properties)
							for Property, Value in next, Properties do
								Object[Property] = Value
							end
						end
						--
						function Utils:Connect(Name, ...)
							local Self = self
							local Table = {
								Connection = Name:Connect(...),
							}
							--
							function Table:Disconnect()
								Self.Connections[Table] = nil
								--
								Table.Connection:Disconnect()
								--
								Table.Connection = nil
								Table.Disconnect = nil
								Table = nil
							end
							--
							Self.Connections[Table] = true
							--
							return Table
						end
						--
						function Utils:Call(Func, ...)
							if Func then
								local Passed, Statement = pcall(Func, ...)
								--
								if not Passed then
									print(string.format("Error: %s", Statement))
									print(debug.traceback())
								end
								--
								return Passed, Statement
							end
						end
						--
						function Utils:Thread(Func, ...)
							Func(...)
						end
						--
						function Utils:Location()
							local Location = Services.UserInputService:GetMouseLocation()
							return Vector2.new(Location.X, Location.Y)
						end
						--
						function Utils:Property(Properties, ...)
							local Indexes = {}
							--
							for Index, Value in next, { ... } do
								Indexes[string.lower(Value)] = true
							end
							--
							for Index2, Value2 in next, Properties do
								if Indexes[string.lower(Index2)] then
									return Value2
								end
							end
						end
						--
						function Utils:Unload()
							task.spawn(function()
								for Index, Value in next, Utils.Connections do
									Index:Disconnect()
								end
								--
								for Index, Value in next, Flags do
									Flags[Index] = nil
									Index = nil
								end
								--
								for Index, Value in next, Menu.Labels do
									Menu.Labels[Index] = nil
									Index = nil
								end
								--
								if Menu.OriginalText then
									for Index, Value in next, Menu.OriginalText do
										Menu.OriginalText[Index] = nil
										Index = nil
									end
								end
								--
								for Index, Value in next, Utils.Screens do
									Index:Remove()
								end
								--
								for Index, Value in next, Utils.Instances do
									Index:Destroy()
								end
							end)
							--
							Env.Unloaded = true
						end
						--
						do -- Table
							function Utils.Table:Count(Array)
								local Count = 0
								--
								for Index, Value in next, Array do
									Count += 1
								end
								--
								return Count
							end
							--
							function Utils.Table:Stringify(Value, Array, ReturnAsArray)
								if typeof(Value) == "table" then
									local Strings = {}
									--
									for Index, Value2 in next, Array do
										if Value[Index] then
											Strings[#Strings + 1] = Value2
										end
									end
									--
									if ReturnAsArray then
										return Strings
									else
										local String = ""
										for Index, Value2 in next, Strings do
											String = String .. Value2 .. (Index == #Strings and "" or ", ")
										end
										return String
									end
								else
									return Array[Value]
								end
							end
							--
							function Utils.Table:State(State, Value)
								if typeof(State) == "table" then
									return (State[Value] or false)
								else
									return (State == Value)
								end
								--
								return false
							end
							--
							function Utils.Table:Modify(State, Value, Min, Max)
								if typeof(State) == "table" then
									local Current = 0
									--
									for Index, Value2 in next, State do
										if Value2 then
											Current = (Current + 1)
										end
									end
									--
									if
										(State[Value] and ((Current - 1) >= Min))
										or (not State[Value] and ((Current + 1) <= Max))
									then
										State[Value] = ((not State[Value]) or nil)
									end
									--
									return State
								else
									return Value
								end
								--
								return 1
							end
							--
							function Utils.Table:DeepCopy(Array)
								local Copy = {}
								--
								for Index, Value in pairs(Array) do
									if type(Value) == "table" then
										Copy[Index] = Utils.Table:DeepCopy(Value)
									else
										Copy[Index] = Value
									end
								end
								--
								return Copy
							end
						end
						--
						do -- Configs
							function Utils.Configs:Update()
								local Self = self
								local List = {}
								--
								for _, File in ipairs(listfiles(string.format("%s/Configs", Menu.FileName))) do
									local FileName = string.gsub(string.match(File, "[^\\]+$"), ".cfg", "")
									List[#List + 1] = FileName
								end
								--
								local IsNew = #List ~= #Self.Local
								if not IsNew then
									for Index, File in ipairs(List) do
										if File ~= Self.Local[Index] then
											IsNew = true
											break
										end
									end
								end
								--
								if IsNew then
									local Raw = {}
									Self.Local = List
									--
									for Index, Value in next, Self.Local do
										Raw[Value] = true
									end
									--
									if Flags["Preset Configurations"] then
										Flags["Preset Configurations"].Options = Raw
										Flags["Preset Configurations"]:Refresh(Raw)
									end
								end
							end
							--
							function Utils.Configs:Get()
								local Config = ""
								--
								for Index, Value in pairs(Flags) do
									if
										Index ~= "Preset Configurations"
										and Index ~= "Preset Name"
										and Index ~= "Watermark"
										and Index ~= "Binds"
										and not string.find(Index, "Holder")
									then
										local Value2 = Value:Get()
										local Final = ""
										--
										if typeof(Value2) == "Color3" then
											local Hue, Saturation, Val = Value2:ToHSV()
											local _, Alpha = Value:Get()
											--
											Final = string.format(
												"rgb(%s,%s,%s,%s)",
												Hue,
												Saturation,
												Val,
												(Alpha and Alpha or 1)
											)
										elseif Value.Mode then
											local Values = Value2
											--
											Final = string.format(
												"key(%s,%s,%s)",
												Value2[1] or "nil",
												Value2[2] or "nil",
												Value.Mode
											)
										elseif Value2 ~= nil then
											if typeof(Value2) == "boolean" then
												Value2 = string.format("bool(%s)", tostring(Value2))
											elseif typeof(Value2) == "table" and not Value.Mode then
												local New = "table("
												--
												for Index2, Value3 in pairs(Value2) do
													New = New .. tostring(Value3) .. ","
												end
												--
												if string.sub(New, #New) == "," then
													New = string.sub(New, 0, #New - 1)
												end
												--
												Value2 = New .. ")"
											elseif typeof(Value2) == "string" then
												Value2 = string.format("string(%s)", Value2)
											elseif typeof(Value2) == "number" then
												Value2 = string.format("number(%s)", Value2)
											end
											--
											Final = Value2
										end
										--
										Config = Config .. Index .. ": " .. tostring(Final) .. "\n"
									end
								end
								--
								return Config
							end
							--
							function Utils.Configs:Set(Config)
								local Table = string.split(Config, "\n")
								local Table2 = {}
								--
								for Index, Value in pairs(Table) do
									local Table3 = string.split(Value, ":")
									--
									if Table3[1] ~= "Preset Configurations" and #Table3 >= 2 then
										local Value = string.sub(Table3[2], 2, #Table3[2])
										--
										if string.sub(Value, 1, 3) == "rgb" then
											local Table4 = string.split(string.sub(Value, 5, #Value - 1), ",")
											--
											Value = {
												Color = Color3.fromHSV(Table4[1], Table4[2], Table4[3]),
												Transparency = Table4[4],
											}
										elseif string.sub(Value, 1, 3) == "key" then
											local Table4 = string.split(string.sub(Value, 5, #Value - 1), ",")
											--
											if Table4[1] == "nil" and Table4[2] == "nil" and Table4[3] ~= "nil" then
												Table4[1] = nil
												Table4[2] = nil
											end
											--
											Value = {
												Table4[1],
												Table4[2],
												Table4[3],
											}
										elseif string.sub(Value, 1, 4) == "bool" then
											local Bool = string.sub(Value, 6, #Value - 1)
											--
											Value = Bool == "true"
										elseif string.sub(Value, 1, 5) == "table" then
											local Table4 = string.split(string.sub(Value, 7, #Value - 1), ",")
											--
											Value = Table4
										elseif string.sub(Value, 1, 6) == "string" then
											local String = string.sub(Value, 8, #Value - 1)
											--
											Value = String
										elseif string.sub(Value, 1, 6) == "number" then
											local Number = tonumber(string.sub(Value, 8, #Value - 1))
											--
											Value = Number
										end
										--
										Table2[Table3[1]] = Value
									end
								end
								--
								if typeof(Table2) == "table" then
									for Index, Value in pairs(Table2) do
										if typeof(Flags[Index]) ~= "nil" then
											if typeof(Value) == "table" and Value.Color then
												Flags[Index]:Set(Value.Color, Value.Transparency)
											else
												Flags[Index]:Set(Value)
											end
										end
									end
								end
							end
						end
					end)()
					--
					LPH_NO_VIRTUALIZE(function()
						do -- Math
							function Utils.Math:Over(Location, Position, Size)
								return (
									(Location.X >= Position.X)
									and (Location.X <= (Position.X + Size.X))
									and (Location.Y >= Position.Y)
									and (Location.Y <= (Position.Y + Size.Y))
								)
							end
							--
							function Utils.Math:Value(Axis, Lowest, Highest)
								return (math.clamp(Axis - Lowest, 0, Highest) / Highest)
							end
							--
							function Utils.Math:Normalise(Value, Decimals, Minimum, Maximum)
								local Rounded = (
									Decimals ~= 1 and (math.floor(Value * Decimals) / Decimals) or math.floor(Value)
								)
								return math.clamp(Rounded, Minimum, Maximum)
							end
							--
							function Utils.Math:Interpolate(Axis, Lowest, Highest, Minimum, Maximum)
								local Self = self
								return (Minimum + (Maximum - Minimum) * Self:Value(Axis, Lowest, Highest))
							end
							--
							function Utils.Math:Percentile(Value, Minimum, Maximum)
								return ((Maximum - Value) / (Maximum - Minimum))
							end
							--
							function Utils.Math:RotateLine(Object, Thickness, From, To)
								local X1, Y1, X2, Y2 = From.X, From.Y, To.X, To.Y
								--
								local CenterX = (X1 + X2) / 2
								local CenterY = (Y1 + Y2) / 2
								--
								local DeltaX = math.abs(X1 - X2) ^ 2
								local DeltaY = math.abs(Y1 - Y2) ^ 2
								--
								local Distance = math.sqrt(DeltaX + DeltaY)
								local Rotation = math.deg(math.atan2(Y1 - Y2, X1 - X2))
								--
								Object.AnchorPoint = Vector2.new(0.5, 0.5)
								Object.Position = UDim2.fromOffset(CenterX, CenterY)
								Object.Size = UDim2.fromOffset(Distance, Thickness)
								Object.Rotation = Rotation
							end
							--
							function Utils.Math:RoundVector(Vector)
								return Vector2.new(math.floor(Vector.X), math.floor(Vector.Y))
							end
						end
						--
						do -- String
							function Utils.String:Normalise(String)
								return string.gsub(string.gsub(String, " ", ""), "\n", "")
							end
							--
							function Utils.String:Surround(String, Combo)
								return string.format("%s%s%s", string.sub(Combo, 1, 1), String, string.sub(Combo, 2, 2))
							end
							--
							function Utils.String:Clamp(String, Length)
								if Flags["Clamp Flag Text"] and Flags["Clamp Flag Text"]:Get() then
									local Split = string.split(String, "\n")
									--
									local Clamped = ""
									--
									for Index, Value2 in pairs(Split) do
										if (Index * 13) <= Length then
											Clamped = Clamped .. Value2 .. (Index == #Split and "" or "\n")
										end
									end
									--
									return (
										Clamped ~= String
											and (Clamped == "" and "" or Clamped:sub(0, #Clamped - 1) .. " ...")
										or Clamped
									)
								end
								return String
							end
						end
					end)()
				end
				--
				do -- Fonts
					Font = getcustomasset and setreadonly(Font, false) or Font
					if getcustomasset then
						function Font:Register(Name, Weight, Style, Asset)
							if not isfile(Asset.Id) then
								writefile(Asset.Id, Asset.Font)
							end

							local Data = {
								name = Name,
								faces = {
									{
										name = "Regular",
										weight = Weight,
										style = Style,
										assetId = getcustomasset(Asset.Id),
									},
								},
							}

							writefile(Name .. ".font", cloneref(game.GetService(game, "HttpService")):JSONEncode(Data))
							return getcustomasset(Name .. ".font")
						end

						function Font:GetRegistry(Name)
							if isfile(Name .. ".font") then
								return getcustomasset(Name .. ".font")
							end
						end
					end

					Font = getcustomasset and setreadonly(Font, true) or Font

					local function RegisterFont(Name, Weight, Style, Asset)
						if getcustomasset then
							return Font.new(Font:Register(Name, Weight, Style, Asset))
						else
							return Font.new("rbxasset://fonts/families/Ubuntu.json", Enum.FontWeight.Bold)
						end
					end

					local MinecraftBase64 =
						"AAEAAAANAIAAAwBQRFNJRwAAAAEAAQX0AAAACEdERUYDBAAkAAEF/AAAAChPUy8y1NZFSAAAANwAAABgY21hcNZo3swAAQp8AAAIOGdhc3AAAAADAAEF7AAAAAhnbHlmEGqBuwAAGjgAANTQaGVhZAZHGCYAAPpgAAAANmhoZWERQQ7WAAAZ9AAAACRobXR4Ywj7gAAA+pgAAAtUbG9jYQEl1TwAAO8IAAALWG1heHAC4wA8AAAaGAAAACBuYW1lomnC7QABBiQAAARWcG9zdN5ON9IAAAE8AAAYuAADBGEBkAAFAAQDAAMAAAABgAMAAwAAAAMAAEwBMgAAAAAEAAAAAAAAAKAAAL9AAGB7AAAAAAAAAABGU1RSAEAAIPsCCQADAAAACoD9wAAAAf8AAAAAB4AJAAAAACAAAQACAAAAAAAAAJgATAAAAAAAAAAAAAAAAAAAAAAAAAAAAtUAAAABAAIAAwAEAAUABgAHAAgACQAKAAsADAANAA4ADwAQABEAEgATABQAFQAWABcAGAAZABoAGwAcAB0AHgAfACAAIQAiACMAJAAlACYAJwAoACkAKgArACwALQAuAC8AMAAxADIAMwA0ADUANgA3ADgAOQA6ADsAPAA9AD4APwBAAEEAQgBDAEQARQBGAEcASABJAEoASwBMAE0ATgBPAFAAUQBSAFMAVABVAFYAVwBYAFkAWgBbAFwAXQBeAF8AYABhAKMAhACFAL0AlgDoAIYAjgCLAJ0AqQCkAIoA2gCDAJMA8gDzAI0AlwCIAMMA3gDxAJ4AqgD1APQA9gCiAK0AyQDHAK4AYgBjAJAAZADLAGUAyADKAM8AzADNAM4A6QBmANMA0ADRAK8AZwDwAJEA1gDUANUAaADrAO0AiQBqAGkAawBtAGwAbgCgAG8AcQBwAHIAcwB1AHQAdgB3AOoAeAB6AHkAewB9AHwAuAChAH8AfgCAAIEA7ADuALoBAgEDAQQBBQEGAQcA/QD+AQgBCQEKAQsA/wEAAQwBDQEOAQEBDwEQAREBEgETARQBFQEWARcBGAEZARoA+AD5ARsBHAEdAR4BHwEgASEBIgEjASQBJQEmAScBKAEpASoA+gDXASsBLAEtAS4BLwEwATEBMgEzATQBNQE2ATcBOAE5AOIA4wE6ATsBPAE9AT4BPwFAAUEBQgFDAUQBRQFGAUcBSACwALEBSQFKAUsBTAFNAU4BTwFQAVEBUgD7APwA5ADlAVMBVAFVAVYBVwFYAVkBWgFbAVwBXQFeAV8BYAFhAWIBYwFkAWUBZgFnAWgAuwFpAWoBawFsAOYA5wFtAW4BbwFwAKYBcQFyAXMBdAF1AXYBdwF4AXkBegF7AXwBfQF+AX8BgAGBAYIBgwGEAYUBhgGHAYgBiQGKANgA4QGLAYwBjQDbANwA3QDgANkA3wGOAY8BkAGRAZIBkwGUAZUBlgGXAZgBmQGaAZsBnAGdAZ4BnwGgAaEBogGjAaQBpQGmAacBqAGpAaoBqwGsAa0BrgGvAbABsQGyAJ8BswG0AbUBtgG3AbgBuQG6AbsBvAG9Ab4BvwHAAcEBwgHDAcQBxQHGAccByACbAckBygHLAcwBzQHOAc8B0AHRAdIB0wHUAdUB1gHXAdgB2QHaAdsB3AHdAd4B3wHgAeEB4gHjAeQB5QHmAecB6AHpAeoB6wHsAe0B7gHvAfAB8QHyAfMB9AH1AfYB9wH4AfkB+gH7AfwB/QH+Af8CAAIBAgICAwIEAgUCBgIHAggCCQIKAgsCDAINAg4CDwIQAhECEgITAhQCFQIWAhcCGAIZAhoCGwIcAh0CHgIfAiACIQIiAiMCJAIlAiYCJwIoAikCKgIrAiwCLQIuAi8CMAIxAjICMwI0AjUCNgI3AjgCOQI6AjsCPAI9Aj4CPwJAAkECQgJDAkQCRQJGAkcCSAJJAkoCSwJMAk0CTgJPAlACUQJSAlMCVAJVAlYCVwJYAlkCWgJbAlwCXQJeAl8CYAJhAmICYwJkAmUCZgJnAmgCaQJqAmsCbAJtAm4CbwJwAnECcgJzAnQCdQJ2AncCeAJ5AnoCewJ8An0CfgJ/AoACgQKCAoMChAKFAoYChwKIAokCigC2ALcAxAKLALQAtQDFAowAggDCAIcCjQCrAMYCjgKPApACkQC+AL8AvAKSApMClAKVApYClwKYApkCmgKbApwCnQKeAp8CoAKhAqICowKkAqUCpgKnAqgCqQKqAqsCrAKtAq4CrwKwAIwCsQKyArMCtAK1ArYCtwK4ArkCugK7ArwCvQK+Ar8CwALBAsICwwLEAsUCxgLHAsgCyQLKAssAqAClAJIAnALMAKcAjwCUAJUCzQLOAs8C0ALRAtIC0wLUAtUC1gLXAtgC2QLaAtsC3ALdAt4AwADBB0FtYWNyb24HYW1hY3JvbgZBYnJldmUGYWJyZXZlB0FvZ29uZWsHYW9nb25lawtDY2lyY3VtZmxleAtjY2lyY3VtZmxleARDZG90BGNkb3QGRGNhcm9uBmRjYXJvbgZEY3JvYXQHRW1hY3JvbgdlbWFjcm9uBkVicmV2ZQZlYnJldmUERWRvdARlZG90B0VvZ29uZWsHZW9nb25lawZFY2Fyb24GZWNhcm9uC0djaXJjdW1mbGV4C2djaXJjdW1mbGV4BEdkb3QEZ2RvdAhHY2VkaWxsYQhnY2VkaWxsYQtIY2lyY3VtZmxleAtoY2lyY3VtZmxleARIYmFyBGhiYXIGSXRpbGRlBml0aWxkZQdJbWFjcm9uB2ltYWNyb24GSWJyZXZlBmlicmV2ZQdJb2dvbmVrB2lvZ29uZWsCSUoCaWoLSmNpcmN1bWZsZXgLamNpcmN1bWZsZXgIS2NlZGlsbGEIa2NlZGlsbGEMa2dyZWVubGFuZGljBkxhY3V0ZQZsYWN1dGUITGNlZGlsbGEIbGNlZGlsbGEGTGNhcm9uBmxjYXJvbgRMZG90BGxkb3QGTmFjdXRlBm5hY3V0ZQhOY2VkaWxsYQhuY2VkaWxsYQZOY2Fyb24GbmNhcm9uC25hcG9zdHJvcGhlA0VuZwNlbmcHT21hY3JvbgdvbWFjcm9uBk9icmV2ZQZvYnJldmUJT2RibGFjdXRlCW9kYmxhY3V0ZQZSYWN1dGUGcmFjdXRlCFJjZWRpbGxhCHJjZWRpbGxhBlJjYXJvbgZyY2Fyb24GU2FjdXRlBnNhY3V0ZQtTY2lyY3VtZmxleAtzY2lyY3VtZmxleAhUY2VkaWxsYQh0Y2VkaWxsYQZUY2Fyb24GdGNhcm9uBFRiYXIEdGJhcgZVdGlsZGUGdXRpbGRlB1VtYWNyb24HdW1hY3JvbgZVYnJldmUGdWJyZXZlBVVyaW5nBXVyaW5nCVVkYmxhY3V0ZQl1ZGJsYWN1dGUHVW9nb25lawd1b2dvbmVrC1djaXJjdW1mbGV4C3djaXJjdW1mbGV4C1ljaXJjdW1mbGV4C3ljaXJjdW1mbGV4BlphY3V0ZQZ6YWN1dGUEWmRvdAR6ZG90BWxvbmdzB0J0b3BiYXIHYnRvcGJhcgVTY2h3YQdac3Ryb2tlB3pzdHJva2UDRXpoB0dzdHJva2UHZ3N0cm9rZQZHY2Fyb24GZ2Nhcm9uBktjYXJvbgZrY2Fyb24HT29nb25lawdvb2dvbmVrCEV6aGNhcm9uCGV6aGNhcm9uCkFyaW5nYWN1dGUKYXJpbmdhY3V0ZQdBRWFjdXRlB2FlYWN1dGULT3NsYXNoYWN1dGULb3NsYXNoYWN1dGUMU2NvbW1hYWNjZW50DHNjb21tYWFjY2VudAtUY29tbWFiZWxvdwt0Y29tbWFiZWxvdwhkb3RsZXNzagVzY2h3YQNlemgQZmlyc3R0b25lY2hpbmVzZRFzZWNvbmR0b25lY2hpbmVzZRFmb3VydGh0b25lY2hpbmVzZRBudW1lcmFsc2lnbmdyZWVrFW51bWVyYWxzaWdubG93ZXJncmVlaw1xdWVzdGlvbmdyZWVrBXRvbm9zDmRpYWx5dGlrYXRvbm9zCkFscGhhdG9ub3MJYW5vdGVsZWlhDEVwc2lsb250b25vcwhFdGF0b25vcwlJb3RhdG9ub3MMT21pY3JvbnRvbm9zDFVwc2lsb250b25vcwpPbWVnYXRvbm9zEWlvdGFkaWVyZXNpc3Rvbm9zBUFscGhhBEJldGEFR2FtbWEKRGVsdGFncmVlawdFcHNpbG9uBFpldGEDRXRhBVRoZXRhBElvdGEFS2FwcGEGTGFtYmRhAk11Ak51AlhpB09taWNyb24CUGkDUmhvBVNpZ21hA1RhdQdVcHNpbG9uA1BoaQNDaGkDUHNpDElvdGFkaWVyZXNpcw9VcHNpbG9uZGllcmVzaXMKYWxwaGF0b25vcwxlcHNpbG9udG9ub3MIZXRhdG9ub3MJaW90YXRvbm9zFHVwc2lsb25kaWVyZXNpc3Rvbm9zBWFscGhhBGJldGEFZ2FtbWEFZGVsdGEHZXBzaWxvbgR6ZXRhA2V0YQV0aGV0YQRpb3RhBWthcHBhBmxhbWJkYQdtdWdyZWVrAm51AnhpB29taWNyb24DcmhvBnNpZ21hMQVzaWdtYQN0YXUHdXBzaWxvbgNwaGkDY2hpA3BzaQVvbWVnYQxpb3RhZGllcmVzaXMPdXBzaWxvbmRpZXJlc2lzDG9taWNyb250b25vcwx1cHNpbG9udG9ub3MKb21lZ2F0b25vcwd1bmkwM0Q3B3VuaTA0MDAJYWZpaTEwMDIzCWFmaWkxMDA1MQlhZmlpMTAwNTIJYWZpaTEwMDUzCWFmaWkxMDA1NAlhZmlpMTAwNTUJYWZpaTEwMDU2CWFmaWkxMDA1NwlhZmlpMTAwNTgJYWZpaTEwMDU5CWFmaWkxMDA2MAlhZmlpMTAwNjEHdW5pMDQwRAlhZmlpMTAwNjIJYWZpaTEwMTQ1CUFjeXJpbGxpYwlhZmlpMTAwMTgJYWZpaTEwMDE5CWFmaWkxMDAyMAlhZmlpMTAwMjEJYWZpaTEwMDIyCWFmaWkxMDAyNAlhZmlpMTAwMjUJYWZpaTEwMDI2CWFmaWkxMDAyNwlhZmlpMTAwMjgJYWZpaTEwMDI5CWFmaWkxMDAzMAlhZmlpMTAwMzEJYWZpaTEwMDMyCWFmaWkxMDAzMwlhZmlpMTAwMzQJYWZpaTEwMDM1CWFmaWkxMDAzNglhZmlpMTAwMzcJYWZpaTEwMDM4CWFmaWkxMDAzOQlhZmlpMTAwNDAJYWZpaTEwMDQxCWFmaWkxMDA0MglhZmlpMTAwNDMJYWZpaTEwMDQ0CWFmaWkxMDA0NQlhZmlpMTAwNDYJYWZpaTEwMDQ3CWFmaWkxMDA0OAlhZmlpMTAwNDkJYWN5cmlsbGljCWFmaWkxMDA2NglhZmlpMTAwNjcJYWZpaTEwMDY4CWFmaWkxMDA2OQlhZmlpMTAwNzAJYWZpaTEwMDcyCWFmaWkxMDA3MwlhZmlpMTAwNzQJYWZpaTEwMDc1CWFmaWkxMDA3NglhZmlpMTAwNzcJYWZpaTEwMDc4CWFmaWkxMDA3OQlhZmlpMTAwODAJYWZpaTEwMDgxCWFmaWkxMDA4MglhZmlpMTAwODMJYWZpaTEwMDg0CWFmaWkxMDA4NQlhZmlpMTAwODYJYWZpaTEwMDg3CWFmaWkxMDA4OAlhZmlpMTAwODkJYWZpaTEwMDkwCWFmaWkxMDA5MQlhZmlpMTAwOTIJYWZpaTEwMDkzCWFmaWkxMDA5NAlhZmlpMTAwOTUJYWZpaTEwMDk2CWFmaWkxMDA5Nwd1bmkwNDUwCWFmaWkxMDA3MQlhZmlpMTAwOTkJYWZpaTEwMTAwCWFmaWkxMDEwMQlhZmlpMTAxMDIJYWZpaTEwMTAzCWFmaWkxMDEwNAlhZmlpMTAxMDUJYWZpaTEwMTA2CWFmaWkxMDEwNwlhZmlpMTAxMDgJYWZpaTEwMTA5B3VuaTA0NUQJYWZpaTEwMTEwCWFmaWkxMDE5MwlhZmlpMTAxNDYJYWZpaTEwMTk0CWFmaWkxMDE0NwlhZmlpMTAxOTUJYWZpaTEwMTQ4CWFmaWkxMDE5Ngd1bmkwNDhBB3VuaTA0OEIHdW5pMDQ4Qwd1bmkwNDhEB3VuaTA0OEUHdW5pMDQ4RglhZmlpMTAwNTAJYWZpaTEwMDk4EUdoZXN0cm9rZWN5cmlsbGljEWdoZXN0cm9rZWN5cmlsbGljFFpoZWRlc2NlbmRlcmN5cmlsbGljFHpoZWRlc2NlbmRlcmN5cmlsbGljE1plZGVzY2VuZGVyY3lyaWxsaWMTemVkZXNjZW5kZXJjeXJpbGxpYxNLYWRlc2NlbmRlcmN5cmlsbGljE2thZGVzY2VuZGVyY3lyaWxsaWMYS2F2ZXJ0aWNhbHN0cm9rZWN5cmlsbGljGGthdmVydGljYWxzdHJva2VjeXJpbGxpYxFLYWJhc2hraXJjeXJpbGxpYxFrYWJhc2hraXJjeXJpbGxpYxNFbmRlc2NlbmRlcmN5cmlsbGljE2VuZGVzY2VuZGVyY3lyaWxsaWMTRXNkZXNjZW5kZXJjeXJpbGxpYxNlc2Rlc2NlbmRlcmN5cmlsbGljEVVzdHJhaWdodGN5cmlsbGljEXVzdHJhaWdodGN5cmlsbGljF1VzdHJhaWdodHN0cm9rZWN5cmlsbGljF3VzdHJhaWdodHN0cm9rZWN5cmlsbGljE0hhZGVzY2VuZGVyY3lyaWxsaWMTaGFkZXNjZW5kZXJjeXJpbGxpYxRDaGVkZXNjZW5kZXJjeXJpbGxpYxRjaGVkZXNjZW5kZXJjeXJpbGxpYxlDaGV2ZXJ0aWNhbHN0cm9rZWN5cmlsbGljGWNoZXZlcnRpY2Fsc3Ryb2tlY3lyaWxsaWMMU2hoYWN5cmlsbGljDHNoaGFjeXJpbGxpYxBwYWxvY2hrYWN5cmlsbGljEFpoZWJyZXZlY3lyaWxsaWMQemhlYnJldmVjeXJpbGxpYwd1bmkwNEM1B3VuaTA0QzYORW5ob29rY3lyaWxsaWMOZW5ob29rY3lyaWxsaWMHdW5pMDRDOQd1bmkwNENBB3VuaTA0Q0QHdW5pMDRDRQd1bmkwNENGEUFkaWVyZXNpc2N5cmlsbGljEWFkaWVyZXNpc2N5cmlsbGljC0FpZWN5cmlsbGljC2FpZWN5cmlsbGljDVNjaHdhY3lyaWxsaWMJYWZpaTEwODQ2D0ltYWNyb25jeXJpbGxpYw9pbWFjcm9uY3lyaWxsaWMRSWRpZXJlc2lzY3lyaWxsaWMRaWRpZXJlc2lzY3lyaWxsaWMRT2RpZXJlc2lzY3lyaWxsaWMRb2RpZXJlc2lzY3lyaWxsaWMPT2JhcnJlZGN5cmlsbGljD29iYXJyZWRjeXJpbGxpYwd1bmkwNEVDB3VuaTA0RUQPVW1hY3JvbmN5cmlsbGljD3VtYWNyb25jeXJpbGxpYxFVZGllcmVzaXNjeXJpbGxpYxF1ZGllcmVzaXNjeXJpbGxpYwZXZ3JhdmUGd2dyYXZlBldhY3V0ZQZ3YWN1dGUJV2RpZXJlc2lzCXdkaWVyZXNpcwd1bmkxRTlFBllncmF2ZQZ5Z3JhdmURcXVvdGVsZWZ0cmV2ZXJzZWQHdW5pMjAxRgd1bmkyMDIzBm1pbnV0ZQZzZWNvbmQNcHJpbWVyZXZlcnNlZAd1bmkyMDM2DHplcm9zdXBlcmlvcgd1bmkyMDcxDGZvdXJzdXBlcmlvcgxmaXZlc3VwZXJpb3ILc2l4c3VwZXJpb3INc2V2ZW5zdXBlcmlvcg1laWdodHN1cGVyaW9yDG5pbmVzdXBlcmlvcgx6ZXJvaW5mZXJpb3ILb25laW5mZXJpb3ILdHdvaW5mZXJpb3INdGhyZWVpbmZlcmlvcgxmb3VyaW5mZXJpb3IMZml2ZWluZmVyaW9yC3NpeGluZmVyaW9yDXNldmVuaW5mZXJpb3INZWlnaHRpbmZlcmlvcgxuaW5laW5mZXJpb3IDd29uBEV1cm8HdW5pMjBBRQd1bmkyMEIxB3VuaTIwQjQHdW5pMjBCOAd1bmkyMEI5B3VuaTIwQkEJYWZpaTYxMjQ4CWFmaWk2MTI4OQlhZmlpNjEzNTIHdW5pMjExNwd1bmkyMTIwCWVzdGltYXRlZAd1bmkyMTUwB3VuaTIxNTEHdW5pMjE1MghvbmV0aGlyZAl0d290aGlyZHMHdW5pMjE1NQd1bmkyMTU2B3VuaTIxNTcHdW5pMjE1OAd1bmkyMTU5B3VuaTIxNUEJb25lZWlnaHRoDHRocmVlZWlnaHRocwtmaXZlZWlnaHRocwxzZXZlbmVpZ2h0aHMJYXJyb3dsZWZ0B2Fycm93dXAKYXJyb3dyaWdodAlhcnJvd2Rvd24JYXJyb3dib3RoCWFycm93dXBkbgthcnJvd3VwbGVmdAxhcnJvd3VwcmlnaHQOYXJyb3dkb3ducmlnaHQNYXJyb3dkb3dubGVmdAhlbXB0eXNldAl0aGVyZWZvcmULYmxhY2tzcXVhcmUGSDIyMDczF2JsYWNrdXBwb2ludGluZ3RyaWFuZ2xlF3doaXRldXBwb2ludGluZ3RyaWFuZ2xlGmJsYWNrcmlnaHRwb2ludGluZ3RyaWFuZ2xlGndoaXRlcmlnaHRwb2ludGluZ3RyaWFuZ2xlBmNpcmNsZQtibGFja2NpcmNsZQZmZW1hbGUEbWFsZQtxdWFydGVybm90ZQttdXNpY2Fsbm90ZRBlaWdodGhub3RlYmVhbWVkDW11c2ljZmxhdHNpZ24HdW5pMjY2RQ5tdXNpY3NoYXJwc2lnbgd1bmkyN0U4B3VuaTI3RTkAAQAACoACQAAACQD+gADACEAAAQAAAAAAAAAAAAAAAAAAAtUAAQAAAtUAPAANAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACAAADwADACQAAAwAHAAARNTMVAxEzEcDAwAPAwMABgAPA/EAAAAAAAgAAB4ACQAkAAAMABwAAEREzETMRMxHAwMAHgAGA/oABgP6AAAAAAAIAAAPAA8AJAAADAB8AAAE1IxUDESM1MzUjNTMRMxEzETMRMxUjFTMVIxEjESMRAkDAwMDAwMDAwMDAwMDAwMAGAMDA/cABgMDAwAGA/oABgP6AwMDA/oABgP6AAAAAAAUAAAPAA8AJAAAHAAsADwATABsAAAE1ITUhFSMVEzUzFSU1IRUlNTMVNTUzNTMVIRUBgP6AAwDAwMD9AAJA/QDAwMABgAPAwMDAwAGAwMDAwMDAwMDAwMDAwAAABwAAA8ADwAkAAAMABwALAA8AEwAXABsAABE1MxUhETMRJREzERE1MxU1ETMRJREzESU1MxXAAkDA/QDAwMD9AMACQMADwMDAAYD+gMABgP6AAYDAwMABgP6AwAGA/oDAwMAAAAgAAAPAA8AJAAADAAcACwAPABsAHwAjACcAABM1IRUzNTMVJREzEQE1MxUBNSM1IzUzNTMRMxEBNTMVMzUzFSU1MxXAAYDAwPxAwAJAwP6AwMDAwMD9wMDAwP6AwAPAwMDAwMABgP6AAYDAwP6AwMDAwP6A/oADAMDAwMDAwMAAAQAAB4AAwAkAAAMAABERMxHAB4ABgP6AAAAAAAUAAAPAAwAJAAADAAcACwAPABMAAAE1IRUlNTMVJREzERE1MxU1NSEVAYABgP3AwP6AwMABgAPAwMDAwMDAAkD9wAJAwMDAwMAAAAAFAAADwAMACQAAAwAHAAsADwATAAARNSEVNTUzFTURMxEBNTMVJTUhFQGAwMD+gMD9wAGAA8DAwMDAwMACQP3AAkDAwMDAwAAFAAAGwAMACQAAAwAHAAsADwATAAARNTMVITUzFSU1IRUlNTMVITUzFcABgMD9wAGA/cDAAYDABsDAwMDAwMDAwMDAwMAAAAABAAADwAPAB4AACwAAAREhNSERMxEhFSERAYD+gAGAwAGA/oADwAGAwAGA/oDA/oAAAAAAAQAAAwAAwAVAAAMAABERMxHAAwACQP3AAAAAAAEAAAVAA8AGAAADAAARNSEVA8AFQMDAAAEAAAPAAMAFQAADAAARETMRwAPAAYD+gAAAAAAFAAADwAPACQAAAwAHAAsADwATAAARNTMVNREzERE1MxU1ETMRETUzFcDAwMDAA8DAwMABgP6AAYDAwMABgP6AAYDAwAAAAAAFAAADwAPACQAAAwAHAA8AFwAbAAATNSEVATUzFQERMxEzFSMVIREjNTM1MxEBNSEVwAJA/oDA/cDAwMACQMDAwP0AAkADwMDAAkDAwP6AA8D9wMDAAkDAwPxAA8DAwAABAAADwAPACQAACwAAETUhESM1MzUzESEVAYDAwMABgAPAwAMAwMD7gMAAAAYAAAPAA8AJAAAFAAkADQARABUAGQAAEREzFSEVATUzFTU1IRUBNTMVBREzEQE1IRXAAwD9AMABgP0AwAJAwP0AAkADwAGAwMABgMDAwMDAAYDAwMABgP6AAYDAwAAAAAcAAAPAA8AJAAADAAcACwAPABMAFwAbAAATNSEVJTUzFSERMxEBNSEVATUzFQURMxEBNSEVwAJA/QDAAkDA/cABgP0AwAJAwP0AAkADwMDAwMDAAYD+gAGAwMABgMDAwAGA/oABgMDAAAAAAAMAAAPAA8AJAAADAAcAEwAAEzUzFTU1MxUTESERMxUhESM1IRHAwMDA/QDAAkDAAYAGwMDAwMDA/EABgAGAwAJAwPrAAAAAAAQAAAPAA8AJAAADAAcACwATAAATNSEVJTUzFSERMxEBESEVIRUhFcACQP0AwAJAwPxAA8D9AAJAA8DAwMDAwAJA/cACQAJAwMDAAAAFAAADwAPACQAAAwAHAA8AEwAXAAATNSEVNREzESERMxUhFSERETUzFTU1IRXAAkDA/EDAAkD9wMABgAPAwMDAAYD+gAMAwMD+gAMAwMDAwMAAAAADAAADwAPACQAAAwAHAA8AAAERMxERNTMVNREhFSMRIREBgMDA/cDAA8ADwAJA/cACQMDAwAGAwAGA/cAAAAAABwAAA8ADwAkAAAMABwALAA8AEwAXABsAABM1IRUlETMRIREzEQE1IRUlETMRIREzEQE1IRXAAkD9AMACQMD9AAJA/QDAAkDA/QACQAPAwMDAAYD+gAGA/oABgMDAwAGA/oABgP6AAYDAwAAABQAAA8ADwAkAAAMABwALABMAFwAAEzUhFTU1MxUBETMRATUhNSERMxEBNSEVwAGAwP0AwAJA/cACQMD9AAJAA8DAwMDAwAJAAYD+gP6AwMABgP0AAwDAwAAAAAACAAADwADACEAAAwAHAAARETMRAxEzEcDAwAPAAYD+gAMAAYD+gAAAAgAAAwAAwAhAAAMABwAAEREzEQMRMxHAwMADAAJA/cADwAGA/oAAAAcAAAPAAwAJAAADAAcACwAPABMAFwAbAAABNTMVJTUzFSU1MxUlNTMVNTUzFTU1MxU1NTMVAkDA/oDA/oDA/oDAwMDAA8DAwMDAwMDAwMDAwMDAwMDAwMDAwAAAAgAABIADwAeAAAMABwAAETUhFQE1IRUDwPxAA8AEgMDAAkDAwAAAAAcAAAPAAwAJAAADAAcACwAPABMAFwAbAAARNTMVNTUzFTU1MxU1NTMVJTUzFSU1MxUlNTMVwMDAwP6AwP6AwP6AwAPAwMDAwMDAwMDAwMDAwMDAwMDAwMAAAAAABgAAA8ADwAkAAAMABwALAA8AEwAXAAABNTMVAzUzFTU1MxUBNTMVBREzEQE1IRUBgMDAwMD9AMACQMD9AAJAA8DAwAGAwMDAwMABgMDAwAGA/oABgMDAAAQAAAPABIAJAAADAAcADwATAAATNSEVJREzETcRIREzETMRATUhFcADwPuAwMABgMDA/EADAAPAwMDAA8D8QMACQP6AAkD9AAMAwMAAAAAAAgAAA8ADwAkAAAsADwAAEREzFSE1MxEjESERETUhFcACQMDA/cACQAPABIDAwPuAAwD9AASAwMAAAAAAAwAAA8ADwAkAAAMABwATAAABETMRAzUzFQERIRUhFSEVIREhFQMAwMDA/EADAP3AAkD9wAJABIACQP3AAwDAwPxABUDAwMD9wMAAAAUAAAPAA8AJAAADAAcACwAPABMAABM1IRU1NTMVIREzEQE1MxUlNSEVwAJAwPxAwAJAwP0AAkADwMDAwMDAA8D8QAMAwMDAwMAAAAACAAADwAPACQAAAwALAAABETMRBREhFSERIRUDAMD8QAMA/cACQASAA8D8QMAFQMD8QMAAAQAAA8ADwAkAAAsAABERIRUhFSEVIREhFQPA/QABgP6AAwADwAVAwMDA/cDAAAAAAQAAA8ADwAkAAAkAABERIRUhFSEVIREDwP0AAYD+gAPABUDAwMD9AAAAAAAEAAADwAPACQAAAwAJAA0AEQAAEzUhFTURITUhESERMxERNSEVwAJA/oACQPxAwAMAA8DAwMACQMD9AAPA/EADwMDAAAEAAAPAA8AJAAALAAARETMRIREzESMRIRHAAkDAwP3AA8AFQP6AAYD6wAMA/QAAAAEAAAPAAkAJAAALAAARNTMRIzUhFSMRMxXAwAJAwMADwMADwMDA/EDAAAAAAwAAA8ADwAkAAAMABwALAAATNSEVJTUzFSERMxHAAkD9AMACQMADwMDAwMDABID7gAAAAAUAAAPAA8AJAAADAAcACwATABcAAAERMxEBNTMVAzUzFQERMxEhFSERATUzFQMAwP6AwMDA/QDAAYD+gAJAwAPAAkD9wAJAwMABgMDA/EAFQP6AwP0ABIDAwAAAAQAAA8ADwAkAAAUAABERMxEhFcADAAPABUD7gMAAAAADAAADwAPACQAAAwALABMAAAE1MxUBETMVMxUjESERIzUzNTMRAYDA/cDAwMACQMDAwAbAwMD9AAVAwMD8QAPAwMD6wAAAAAADAAADwAPACQAAAwALABMAAAE1MxUBETMVMxUjESERIzUzETMRAYDA/cDAwMACQMDAwAbAwMD9AAVAwMD8QAJAwAJA+sAAAAAEAAADwAPACQAAAwAHAAsADwAAEzUhFSURMxEhETMRATUhFcACQP0AwAJAwP0AAkADwMDAwAPA/EADwPxAA8DAwAAAAAIAAAPAA8AJAAADAA0AAAE1MxUBESEVIRUhFSERAwDA/EADAP3AAkD9wAeAwMD8QAVAwMDA/QAABgAAA8ADwAkAAAMABwALAA8AEwAXAAATNSEVMzUzFSU1MxUhETMRJREzEQE1IRXAAYDAwP6AwP0AwAJAwP0AAkADwMDAwMDAwMADwPxAwAMA/QADAMDAAAMAAAPAA8AJAAADAAcAEQAAAREzEQM1MxUBESEVIRUhFSERAwDAwMD8QAMA/cACQP3AA8ADAP0AA8DAwPxABUDAwMD9AAAAAAYAAAPAA8AJAAADAAcACwAPABMAFwAAEzUhFSU1MxUhETMRATUhFSU1MxU1NSEVwAJA/QDAAkDA/QACQP0AwAMAA8DAwMDAwAJA/cACQMDAwMDAwMDAAAABAAADwAPACQAABwAAAREhNSEVIREBgP6AA8D+gAPABIDAwPuAAAAAAwAAA8ADwAkAAAMABwALAAATNSEVJREzESERMxHAAkD9AMACQMADwMDAwASA+4AEgPuAAAUAAAPAA8AJAAADAAcACwAPABMAAAE1MxUlETMRMxEzEQERMxEhETMRAYDA/oDAwMD9AMACQMADwMDAwAGA/oABgP6AAYADAP0AAwD9AAAAAwAAA8ADwAkAAAMACwATAAABNTMVAREzETMVIxUhNSM1MxEzEQGAwP3AwMDAAkDAwMAFQMDA/oAFQPxAwMDAwAPA+sAAAAAACQAAA8ADwAkAAAMABwALAA8AEwAXABsAHwAjAAARETMRIREzEQE1MxUzNTMVJTUzFSU1MxUzNTMVJTUzFSE1MxXAAkDA/QDAwMD+gMD+gMDAwP0AwAJAwAPAAkD9wAJA/cACQMDAwMDAwMDAwMDAwMDAwMDAAAAABQAAA8ADwAkAAAMABwALAA8AEwAAAREzEQE1MxUzNTMVJTUzFSE1MxUBgMD+gMDAwP0AwAJAwAPAA8D8QAPAwMDAwMDAwMDAAAAAAAUAAAPAA8AJAAAFAAkADQARABcAABERMxUhFQE1MxU1NTMVNTUzFTU1ITUhEcADAP0AwMDA/QADwAPAAYDAwAGAwMDAwMDAwMDAwMD+gAAAAQAAA8ACQAkAAAcAABERIRUhESEVAkD+gAGAA8AFQMD8QMAABQAAA8ADwAkAAAMABwALAA8AEwAAATUzFSURMxEBNTMVJREzEQE1MxUDAMD+gMD+gMD+gMD+gMADwMDAwAGA/oABgMDAwAGA/oABgMDAAAABAAADwAJACQAABwAAETUhESE1IREBgP6AAkADwMADwMD6wAAFAAAGwAPACQAAAwAHAAsADwATAAARNTMVITUzFSU1MxUzNTMVJTUzFcACQMD9AMDAwP6AwAbAwMDAwMDAwMDAwMDAAAEAAAMAA8ADwAADAAARNSEVA8ADAMDAAAIAAAhAAYAJwAADAAcAABM1MxUlNTMVwMD+gMAIQMDAwMDAAAMAAAPAA8AHgAADAA0AEQAAETUzFRU1ITUhNSE1MxEBNSEVwAJA/cACQMD9AAJABIDAwMDAwMDA/QADAMDAAAADAAADwAPACQAAAwAHABEAAAERMxEBNSEVAREzETMVIxEhFQMAwP3AAYD9AMDAwAJABIACQP3AAkDAwP0ABUD9wMD+gMAAAAAFAAADwAPAB4AAAwAHAAsADwATAAATNSEVNTUzFSERMxEBNTMVJTUhFcACQMD8QMACQMD9AAJAA8DAwMDAwAJA/cABgMDAwMDAAAAAAwAAA8ADwAkAAAMABwARAAARETMRETUhFQE1IREjNTMRMxHAAYD+gAJAwMDABIACQP3AAkDAwP0AwAGAwAJA+sAAAAADAAADwAPAB4AAAwANABEAABM1IRUlETMVITUzESEVETUhFcADAPxAwAJAwP0AAkADwMDAwAJAwMD+gMACQMDAAAAAAAIAAAPAAwAJAAALAA8AABMRIzUzNTMVIRUhERE1IRXAwMDAAYD+gAGAA8ADAMDAwMD9AASAwMAAAwAAAwADwAeAAAMABwARAAARNSEVAREzEQE1ITUhESE1IREDAP0AwAJA/cACQP3AAwADAMDAAkABgP6A/oDAwAGAwPxAAAAAAwAAA8ADwAkAAAMABwAPAAABETMRATUhFQERMxEzFSMRAwDA/cABgP0AwMDAA8ADAP0AAwDAwP0ABUD9wMD9wAAAAAACAAADwADACQAAAwAHAAARETMRAzUzFcDAwAPAA8D8QASAwMAAAAAABAAAAwADwAkAAAMABwALAA8AABM1IRUlETMRIREzEQM1MxXAAkD9AMACQMDAwAMAwMDAAYD+gAPA/EAEgMDAAAUAAAPAAwAJAAADAAcACwAPABcAAAE1MxUlNTMVAzUzFTU1MxUBETMRMxUjEQJAwP6AwMDAwP0AwMDAA8DAwMDAwAGAwMDAwMD9AAVA/QDA/oAAAAIAAAPAAYAJAAADAAcAABM1MxUlETMRwMD+gMADwMDAwASA+4AAAAAEAAADwAPAB4AAAwAHAA0AEQAAAREzERMRMxEhESEVIxEBNTMVAYDAwMD8QAGAwAGAwAVAAYD+gP6AAwD9AAPAwP0AAwDAwAACAAADwAPAB4AAAwAJAAABETMRIREhFSERAwDA/EADAP3AA8ADAP0AA8DA/QAAAAAEAAADwAPAB4AAAwAHAAsADwAAEzUhFSURMxEhETMRATUhFcACQP0AwAJAwP0AAkADwMDAwAJA/cACQP3AAkDAwAAAAAMAAAMAA8AHgAADAA8AEwAAAREzEQERMxUzFSMVIRUhERM1IRUDAMD8QMDAwAJA/cDAAYAFQAGA/oD9wASAwMDAwP6AA8DAwAAAAAADAAADAAPAB4AAAwAHABMAABERMxERNSEVExEhNSE1IzUzNTMRwAGAwP3AAkDAwMAFQAGA/oABgMDA/EABgMDAwMD7gAAAAAADAAADwAPAB4AAAwALAA8AAAE1MxUBETMVMxUjERM1IRUDAMD8QMDAwMABgAYAwMD9wAPAwMD9wAMAwMAAAAAABQAAA8ADwAeAAAMABwALAA8AEwAAETUhFTU1MxUlNSEVJTUzFTU1IRUDAMD9AAJA/QDAAwADwMDAwMDAwMDAwMDAwMDAAAAAAgAAA8ACQAkAAAMADwAAATUzFSURIzUzNTMVMxUjEQGAwP6AwMDAwMADwMDAwAMAwMDAwP0AAAACAAADwAPAB4AAAwAJAAARETMRFTUhETMRwAJAwASAAwD9AMDAAwD8QAAAAAUAAAPAA8AHgAADAAcACwAPABMAAAE1MxUlNTMVMzUzFSURMxEhETMRAYDA/oDAwMD9AMACQMADwMDAwMDAwMDAAkD9wAJA/cAAAAACAAADwAPAB4AAAwANAAARETMRFTUzETMRMxEzEcDAwMDABIADAP0AwMABgP6AAwD8QAAACQAAA8ADwAeAAAMABwALAA8AEwAXABsAHwAjAAARNTMVITUzFSU1MxUzNTMVJTUzFSU1MxUzNTMVJTUzFSE1MxXAAkDA/QDAwMD+gMD+gMDAwP0AwAJAwAPAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwAAAAAADAAADAAPAB4AAAwAHAA8AABE1IRUBETMRATUhNSERMxEDAP0AwAJA/cACQMADAMDAAkACQP3A/oDAwAJA/EAAAwAAA8ADwAeAAAcACwATAAARNTM1MxUhFQE1MxU1NSE1IRUjFcDAAkD9wMD9wAPAwAPAwMDAwAGAwMDAwMDAwAAAAAAFAAADwAMACQAAAwAHAAsADwATAAABNSEVJREzEQE1MxU1ETMRETUhFQGAAYD9wMD+gMDAAYADwMDAwAGA/oABgMDAwAGA/oABgMDAAAAAAAEAAAMAAMAJAAADAAARETMRwAMABgD6AAAAAAAFAAADwAMACQAAAwAHAAsADwATAAARNSEVNREzERE1MxUlETMRATUhFQGAwMD+gMD9wAGAA8DAwMABgP6AAYDAwMABgP6AAYDAwAAABAAAB4AEgAkAAAMABwALAA8AABE1MxUhNSEVJTUhFSE1MxXAAYABgP0AAYABgMAHgMDAwMDAwMDAwAAAAgAAA8AAwAkAAAMABwAAEREzEQM1MxXAwMADwAPA/EAEgMDAAAAAAAMAAAPAAwAHgAAHAAsAEwAAATUjNSEVIxUBNTMVNTUzNTMVMxUBgMACQMD9wMDAwMADwMDAwMABgMDAwMDAwMAAAAAAAgAAA8ADwAkAAA8AEwAAETUzESM1MxEzESEVIREhFQE1IRXAwMDAAYD+gAJA/cACQAPAwAGAwAGA/oDA/oDABIDAwAAGAAAEgAPACEAAAwAHAAsADwATABcAABE1MxUhNTMVATUjFQcRIREBNTMVITUzFcACQMD+gMDAAkD9AMACQMAEgMDAwMABgMDAwAJA/cACQMDAwMAAAAAABQAAA8ADwAkAABMAFwAbAB8AIwAAATUjNTM1IzUzNTMVMxUjFTMVIxUBNTMVMzUzFSU1MxUhNTMVAYDAwMDAwMDAwMD+gMDAwP0AwAJAwAPAwMDAwMDAwMDAwAPAwMDAwMDAwMDAAAACAAADwADACQAAAwAHAAARETMRAxEzEcDAwAPAAkD9wAMAAkD9wAAACAAAA8ADwAkAAAMABwALAA8AEwAXABsAHwAAETUhFTU1MxUlNSEVJTUzFSE1MxUlNSEVJTUzFTU1IRUDAMD9AAJA/QDAAkDA/QACQP0AwAMAA8DAwMDAwMDAwMDAwMDAwMDAwMDAwMDAAAAAAgDACEADAAkAAAMABwAAEzUzFTM1MxXAwMDACEDAwMDAAAAABwAAA8AFQAkAAAMABwALAA8AEwAXABsAABM1IRUBNSEVJTUzFTU1IRUBETMRIREzEQE1IRXAA8D9wAGA/cDAAYD8QMADwMD7gAPAA8DAwAGAwMDAwMDAwMD9wAPA/EADwPxAA8DAwAADAAAEgAJACQAAAwAHABMAABE1IRUBNTMVFTUzNSM1IzUhFTMRAkD9wMDAwMABgMAEgMDAAkDAwMDAwMDAwP3AAAAAAAYAAAPAAwAGwAADAAcACwAPABMAFwAAEzUzFTM1MxUlETMRMxEzEQE1MxUzNTMVwMDAwP0AwMDA/oDAwMADwMDAwMDAAYD+gAGA/oABgMDAwMAAAAEAAAVAAwAHgAAFAAABESE1IRECQP3AAwAFQAGAwP3AAAAAAAYAAAPABUAJAAADAAcADwATABcAGwAAEzUhFQE1MxUhESEVIxUjFQURMxEhETMRATUhFcADwP6AwP3AAkDAwP3AwAPAwPuAA8ADwMDAAYDAwAJAwMDAwAPA/EADwPxAA8DAwAABAAAIQAPACQAAAwAAETUhFQPACEDAwAAEAAAGwAJACQAAAwAHAAsADwAAEzUzFSU1MxUzNTMVJTUzFcDA/oDAwMD+gMAGwMDAwMDAwMDAwMAAAAACAAADwAPACQAAAwAPAAARNSEVAREhNSERMxEhFSERA8D9wP6AAYDAAYD+gAPAwMABgAGAwAGA/oDA/oAAAAAAAwAABgACQAkAAAcACwAPAAARNTM1MxUzFQM1MxUlNSEVwMDAwMD9wAGABgDAwMDAAYDAwMDAwAACAAAGAAJACQAAAwANAAARNSEVNTUjNSM1IRUzEQGAwMABgMAGAMDAwMDAwMD+gAACAAAIQAGACcAAAwAHAAARNTMVNTUzFcDACEDAwMDAwAAAAAACAAADAAPAB4AABwAPAAARETMRIRUhFSU1IzUzETMRwAGA/oACQMDAwAMABID9AMDAwMDAAkD8QAAAAAABAAADwAPACQAACwAAAREjESE1IxEzNSERAwDA/oDAwAMAA8AEgP3AwAGAwPrAAAABAAAFQADABgAAAwAAETUzFcAFQMDAAAABAMACQAJAA8AABQAAEzUzNTMRwMDAAkDAwP6AAAAAAAEAAAYAAkAJAAALAAARNTM1IzUzNTMRMxXAwMDAwAYAwMDAwP3AwAAFAAAEgAJACQAAAwAHAAsADwATAAARNSEVATUzFSURMxEzETMRATUzFQJA/oDA/oDAwMD+gMAEgMDAAYDAwMABgP6AAYD+gAGAwMAABgAAA8ADAAbAAAMABwALAA8AEwAXAAARNTMVMzUzFSURMxEzETMRATUzFTM1MxXAwMD+gMDAwP0AwMDAA8DAwMDAwAGA/oABgP6AAYDAwMDAAAAABwAAA8AGAAkAAAMABwALABcAGwAnACsAAAE1MxU1ETMRETUzFQE1IREzFTM1IzUhEQERMxEFNTM1IzUzNTMRMxUBNTMVAYDAwMABgP6AwMDAAYD9wMD7gMDAwMDAAkDAA8DAwMABgP6AAYDAwP3AwAGAwMDA/QADAAGA/oDAwMDAwP3AwAJAwMAAAAAHAAADwAYACQAAAwALAA8AEwAbACcAKwAAATUzFSE1MzUzFTMVJREzESU1MxUlNTMRMxEzFSE1MzUjNTM1MxEzFQE1MxUBgMABgMDAwPxAwAJAwP0AwMDA+sDAwMDAwAJAwAPAwMDAwMDAwAGA/oDAwMDAwAGA/oDAwMDAwP3AwAJAwMAAAAAACAAAA8AGAAkAAAMABwALAA8AGwAfACkALQAAATUzFTURMxEBNSEVITUzFQE1IREzFTM1IzUhEQERMxEhNSM1IzUhFTMRATUzFQGAwMD9AAGAAYDAAYD+gMDAwAGA/cDA/QDAwAGAwAJAwAPAwMDAAYD+gAGAwMDAwP3AwAGAwMDA/QADAAGA/oDAwMDA/oABgMDAAAYAAAPAA8AJAAADAAcACwAPABMAFwAAEzUhFTU1MxUhETMRETUzFTU1MxUDNTMVwAJAwPxAwMDAwMADwMDAwMDAAYD+gAGAwMDAwMABgMDAAAAAAAMAAAPAA8AKgAALABMAFwAAEREzFSE1MxEjESERETUzNTMVMxUBNTMVwAJAwMD9wMDAwP3AwAPABIDAwPuAAwD9AASAwMDAwAGAwMAAAAAAAwAAA8ADwAqAAAsAEwAXAAARETMVITUzESMRIRERNTM1MxUzFQM1MxXAAkDAwP3AwMDAwMADwASAwMD7gAMA/QAEgMDAwMABgMDAAAMAAAPAA8AKgAALABMAFwAAEREzFSE1MxEjESEREREzFTM1MxEBNTMVwAJAwMD9wMDAwP6AwAPABIDAwPuAAwD9AASAAYDAwP6AAYDAwAAABAAAA8ADwAqAAAsADwAZAB0AABERMxUhNTMRIxEhEQM1MxUVNTM1IzUhFTMRETUzFcACQMDA/cDAwMDAAYDAwAPABIDAwPuAAwD9AAVAwMDAwMDAwP6AAYDAwAAEAAADwAPACoAACwAPABMAFwAAEREzFSE1MxEjESERETUhFQE1MxUzNTMVwAJAwMD9wAJA/cDAwMADwASAwMD7gAMA/QAEgMDAAYDAwMDAAAMAAAPAA8AKgAALAA8AEwAAEREzFSE1MxEjESERATUjFQcRIRHAAkDAwP3AAYDAwAJAA8AEgMDA+4ADAP0ABUDAwMACQP3AAAEAAAPABsAJAAAVAAARETMVITUhNSEVIRUhFSERIRUhESERwAJA/cAGAP0AAYD+gAMA/ED9wAPABIDAwMDAwMD9wMADAP0AAAUAAAJAA8AJAAAJAA0AEQAVABkAABM1MzUjNSEVIxETNTMVIREzEQE1MxUlNSEVwMDAAkDAwMD8QMACQMD9AAJAAkDAwMDA/oACQMDAA8D8QAMAwMDAwMAAAAAAAgAAA8ADwAqAAA8AEwAAEREhNTMVIRUhFSEVIREhFQE1MxUBgMABgP0AAYD+gAMA/QDAA8AFQMDAwMDA/cDABgDAwAACAAADwAPACoAADwATAAARESE1MxUhFSEVIRUhESEVATUzFQGAwAGA/QABgP6AAwD+gMADwAVAwMDAwMD9wMAGAMDAAAIAAAPAA8AKgAATABcAABERMzUzFTM1MxUzFSEVIRUhESEVATUzFcDAwMDA/QABgP6AAwD9wMADwAVAwMDAwMDAwP3AwAYAwMAAAAADAAADwAPACoAACwAPABMAABERIRUhFSEVIREhFQE1MxUzNTMVA8D9AAGA/oADAP0AwMDAA8AFQMDAwP3AwAYAwMDAwAAAAAACAAADwAJACoAADwATAAARNTMRIzUzNTMVMxUjETMVATUzFcDAwMDAwMD9wMADwMADwMDAwMD8QMAGAMDAAAAAAgAAA8ACQAqAAA8AEwAAETUzESM1MzUzFTMVIxEzFQM1MxXAwMDAwMDAwMADwMADwMDAwMD8QMAGAMDAAAAAAAIAAAPAAkAKgAAPABMAABE1MxEjETMVMzUzESMRMxUBNTMVwMDAwMDAwP6AwAPAwAPAAYDAwP6A/EDABgDAwAADAAADwAJACoAACwAPABMAABE1MxEjNSEVIxEzFQE1MxUzNTMVwMACQMDA/cDAwMADwMADwMDA/EDABgDAwMDAAAAAAAIAAAPABIAJAAADABMAAAERMxEFESM1MxEhFSEVMxUjESEVA8DA/EDAwAMA/cDAwAJABIADwPxAwAMAwAGAwMDA/cDAAAAAAAUAAAPAA8AKgAADAAsAEwAbAB8AAAE1MxUTESM1MxEzESERMxEzFSMREzUjNSEVMxU1NTMVAYDAwMDAwPxAwMDAwMABgMDABsDAwP0AAkDAAkD6wAYA/oDA/EAFQMDAwMDAwMAAAAAABQAAA8ADwAqAAAMABwALABMAFwAAEzUhFSURMxEhETMRATUzNTMVMxUBNTMVwAJA/QDAAkDA/QDAwMD9wMADwMDAwAPA/EADwPxAA8DAwMDAAYDAwAAAAAUAAAPAA8AKgAADAAcACwATABcAABM1IRUlETMRIREzEQE1MzUzFTMVAzUzFcACQP0AwAJAwP0AwMDAwMADwMDAwAPA/EADwPxAA8DAwMDAAYDAwAAAAAAFAAADwAPACoAAAwAHAAsAEwAXAAATNSEVJREzESERMxEBETMVMzUzEQE1MxXAAkD9AMACQMD9AMDAwP6AwAPAwMDAA8D8QAPA/EADwAGAwMD+gAGAwMAABgAAA8ADwAqAAAMABwALAA8AGQAdAAATNSEVJREzESERMxEBNTMVFTUzNSM1IRUzERE1MxXAAkD9AMACQMD8QMDAwAGAwMADwMDAwAPA/EADwPxABIDAwMDAwMDA/oABgMDAAAYAAAPAA8AKgAADAAcACwAPABMAFwAAEzUhFSURMxEhETMRATUhFQE1MxUzNTMVwAJA/QDAAkDA/QACQP3AwMDAA8DAwMADwPxAA8D8QAPAwMABgMDAwMAAAAAABQAAA8ADAAbAAAMABwALAA8AEwAAETUzFSE1MxUlESERATUzFSE1MxXAAYDA/cABgP3AwAGAwAPAwMDAwMABgP6AAYDAwMDAAAAAAAcAAAPAA8AJAAADAAcACwATABsAHwAjAAARNTMVMzUhFQE1MxUBNSMRMxEzESERIxEzFTMRATUhFTM1MxXAwAGA/oDA/oDAwMABgMDAwP0AAYDAwAPAwMDAwAJAwMD+gMADAP3A/oACQAGAwP0AA8DAwMDAAAAAAAUAAAPAA8AKgAADAAcACwAPABMAABM1IRUlETMRIREzEQE1MxUlNTMVwAJA/QDAAkDA/cDA/oDAA8DAwMAEgPuABID7gASAwMDAwMAAAAAABQAAA8ADwAqAAAMABwALAA8AEwAAEzUhFSURMxEhETMRATUzFTU1MxXAAkD9AMACQMD9wMDAA8DAwMAEgPuABID7gASAwMDAwMAAAAYAAAPAA8AKgAADAAcACwAPABMAFwAAEzUhFSURMxEhETMRATUzFTM1MxUlNTMVwAJA/QDAAkDA/QDAwMD+gMADwMDAwASA+4AEgPuABIDAwMDAwMDAAAAFAAADwAPACoAAAwAHAAsADwATAAATNSEVJREzESERMxEBNTMVMzUzFcACQP0AwAJAwP0AwMDAA8DAwMAEgPuABID7gAVAwMDAwAAABwAAA8ADwAqAAAMABwALAA8AEwAXABsAAAERMxEBNTMVMzUzFSU1MxUhNTMVJTUzFTU1MxUBgMD+gMDAwP0AwAJAwP3AwMADwAPA/EADwMDAwMDAwMDAwMDAwMDAwAAAAgAAA8ADwAkAAAMADwAAATUzFQERMxUhFSEVIRUhEQMAwPxAwAJA/cACQP3ABsDAwP0ABUDAwMDA/cAABQAAA8ADwAkAAAMABwALABEAFQAAATUhFTURMxEFETMREzUzNTMRATUhFQGAAYDA/EDAwMDA/cABgAPAwMDAAkD9wMAEgPuAAwDAwP6AAYDAwAAABQAAA8ADwAnAAAMADQARABUAGQAAETUzFRU1ITUhNSE1MxEBNSEVATUzFSU1MxXAAkD9wAJAwP0AAkD+gMD+gMAEgMDAwMDAwMD9AAMAwMABgMDAwMDAAAUAAAPAA8AJwAADAA0AEQAVABkAABE1MxUVNSE1ITUhNTMRATUhFQE1MxU1NTMVwAJA/cACQMD9AAJA/oDAwASAwMDAwMDAwP0AAwDAwAGAwMDAwMAAAAAGAAADwAPACcAAAwANABEAFQAZAB0AABE1MxUVNSE1ITUhNTMRATUhFQE1MxUzNTMVJTUzFcACQP3AAkDA/QACQP3AwMDA/oDABIDAwMDAwMDA/QADAMDAAYDAwMDAwMDAAAAABgAAA8ADwAnAAAMADQARABUAHQAhAAARNTMVFTUhNSE1ITUzEQE1IRUBNTMVMzUjNSEVMxU1NTMVwAJA/cACQMD9AAJA/QDAwMABgMDABIDAwMDAwMDA/QADAMDAAYDAwMDAwMDAwMAAAAAABQAAA8ADwAkAAAMADQARABUAGQAAETUzFRU1ITUhNSE1MxEBNSEVATUzFTM1MxXAAkD9wAJAwP0AAkD9wMDAwASAwMDAwMDAwP0AAwDAwAGAwMDAwAAAAAUAAAPAA8AKgAADAA0AEQAVABkAABE1MxUVNSE1ITUhNTMRATUhFQM1IxUHESERwAJA/cACQMD9AAJAwMDAAkAEgMDAwMDAwMD9AAMAwMACQMDAwAJA/cAAAAAABgAAA8AGwAeAAAMABwALABkAHQAhAAATNSEVMzUhFSU1MxUhNSE1ITUzFSE1MxEhFQE1IRUzNSEVwAJAwAJA+gDAAkD9wAJAwAJAwP0A/QACQMACQAPAwMDAwMDAwMDAwMDA/oDAAkDAwMDAAAAAAAUAAAJAA8AHgAAJAA0AEQAVABkAABM1MzUjNSEVIxETNTMVIREzEQE1MxUlNSEVwMDAAkDAwMD8QMACQMD9AAJAAkDAwMDA/oACQMDAAkD9wAGAwMDAwMAAAAAABQAAA8ADwAnAAAMADQARABUAGQAAEzUhFSURMxUhNTMRIRURNSEVATUzFSU1MxXAAwD8QMACQMD9AAJA/oDA/oDAA8DAwMACQMDA/oDAAkDAwAGAwMDAwMAAAAAFAAADwAPACcAAAwANABEAFQAZAAATNSEVJREzFSE1MxEhFRE1IRUBNTMVNTUzFcADAPxAwAJAwP0AAkD+gMDAA8DAwMACQMDA/oDAAkDAwAGAwMDAwMAABgAAA8ADwAnAAAMADQARABUAGQAdAAATNSEVJREzFSE1MxEhFRE1IRUBNTMVMzUzFSU1MxXAAwD8QMACQMD9AAJA/cDAwMD+gMADwMDAwAJAwMD+gMACQMDAAYDAwMDAwMDAAAUAAAPAA8AJAAADAA0AEQAVABkAABM1IRUlETMVITUzESEVETUhFQE1MxUzNTMVwAMA/EDAAkDA/QACQP3AwMDAA8DAwMACQMDA/oDAAkDAwAGAwMDAwAAD/0ADwADACcAAAwAHAAsAABERMxEDNTMVJTUzFcDAwP6AwAPAA8D8QASAwMDAwMAAAAAAAwAAA8ABgAnAAAMABwALAAARETMRAzUzFTU1MxXAwMDAA8ADwPxABIDAwMDAwAAABP9AA8ABgAnAAAMABwALAA8AABERMxEBNTMVMzUzFSU1MxXA/oDAwMD+gMADwAPA/EAEgMDAwMDAwMAAA/9AA8ABgAkAAAMABwALAAARETMRATUzFTM1MxXA/oDAwMADwAPA/EAEgMDAwMAABwAAA8ADwAkAAAMABwALABMAFwAbAB8AABM1IRUlNTMVEzUzFRM1ITUhETMRATUzFSU1MxUzNTMVwAJA/QDAwMDA/cACQMD+gMD+gMDAwAPAwMDAwMACQMDA/cDAwAGA/QADAMDAwMDAwMAABQAAA8ADwAnAAAMACQANABUAGQAAAREzESERIRUhEQM1MxUzNSM1IRUzFTU1MxUDAMD8QAMA/cDAwMDAAYDAwAPAAwD9AAPAwP0ABIDAwMDAwMDAwMAAAAYAAAPAA8AJwAADAAcACwAPABMAFwAAEzUhFSURMxEhETMRATUhFQE1MxUlNTMVwAJA/QDAAkDA/QACQP6AwP6AwAPAwMDAAkD9wAJA/cACQMDAAYDAwMDAwAAABgAAA8ADwAnAAAMABwALAA8AEwAXAAATNSEVJREzESERMxEBNSEVATUzFTU1MxXAAkD9AMACQMD9AAJA/oDAwAPAwMDAAkD9wAJA/cACQMDAAYDAwMDAwAAAAAAHAAADwAPACcAAAwAHAAsADwATABcAGwAAEzUhFSURMxEhETMRATUhFQE1MxUzNTMVJTUzFcACQP0AwAJAwP0AAkD9wMDAwP6AwAPAwMDAAkD9wAJA/cACQMDAAYDAwMDAwMDAAAAAAAcAAAPAA8AJwAADAAcACwAPABMAGwAfAAATNSEVJREzESERMxEBNSEVATUzFTM1IzUhFTMVNTUzFcACQP0AwAJAwP0AAkD9AMDAwAGAwMADwMDAwAJA/cACQP3AAkDAwAGAwMDAwMDAwMDAAAYAAAPAA8AJAAADAAcACwAPABMAFwAAEzUhFSURMxEhETMRATUhFQE1MxUzNTMVwAJA/QDAAkDA/QACQP3AwMDAA8DAwMACQP3AAkD9wAJAwMABgMDAwMAAAAAAAwAAA8ADwAeAAAMABwALAAABNTMVATUhFQE1MxUBgMD9wAPA/cDAA8DAwAGAwMABgMDAAAkAAAPAA8AHgAADAAcACwAPABMAFwAbAB8AIwAAETUzFTM1IRUlNTMVNTUzFRcRMxElETMRJTUzFSU1IRUzNTMVwMABgP3AwMDAwPxAwAGAwP3AAYDAwAPAwMDAwMDAwMDAwMABgP6AwAGA/oDAwMDAwMDAwAAAAAQAAAPAA8AJwAADAAkADQARAAARETMRFTUhETMRATUzFSU1MxXAAkDA/cDA/oDABIADAP0AwMADAPxABIDAwMDAwAAABAAAA8ADwAnAAAMACQANABEAABERMxEVNSERMxEBNTMVNTUzFcACQMD9wMDABIADAP0AwMADAPxABIDAwMDAwAAAAAAFAAADwAPACcAAAwAJAA0AEQAVAAARETMRFTUhETMRATUzFTM1MxUlNTMVwAJAwP0AwMDA/oDABIADAP0AwMADAPxABIDAwMDAwMDAAAAAAAQAAAPAA8AJAAADAAkADQARAAARETMRFTUhETMRATUzFTM1MxXAAkDA/QDAwMAEgAMA/QDAwAMA/EAEgMDAwMAAAAAABQAAAwADwAnAAAMABwAPABMAFwAAETUhFQERMxEBNSE1IREzEQE1MxU1NTMVAwD9AMACQP3AAkDA/cDAwAMAwMACQAJA/cD+gMDAAkD8QASAwMDAwMAAAAMAAAMAA8AJAAADAAcAEwAAAREzEQE1IRUBETMRMxUjFSEVIREDAMD9wAGA/QDAwMACQP3ABUABgP6AAYDAwPxABgD9wMDAwP6AAAADAAADAAPAB4AAAwAHAA8AABE1IRUBETMRATUhNSERMxEDAP0AwAJA/cACQMADAMDAAkACQP3A/oDAwAJA/EAAAwAAA8ADwAqAAAsADwATAAARETMVITUzESMRIRERNSEVATUhFcACQMDA/cACQP3AAkADwASAwMD7gAMA/QAEgMDAAYDAwAAABAAAA8ADwAkAAAMADQARABUAABE1MxUVNSE1ITUhNTMRATUhFQE1IRXAAkD9wAJAwP0AAkD9wAJABIDAwMDAwMDA/QADAMDAAYDAwAAAAAACAAADwAPACoAACwATAAARETMVITUzESMRIRERETMVMzUzEcACQMDA/cDAwMADwASAwMD7gAMA/QAEgAJAwMD9wAAAAAQAAAPAA8AJwAADAA0AEQAZAAARNTMVFTUhNSE1ITUzEQE1IRUBETMVMzUzEcACQP3AAkDA/QACQP3AwMDABIDAwMDAwMDA/QADAMDAAYABgMDA/oAAAAADAAACQAPACQAABQARABUAAAERMxUzFQERMxUhNTMRIxEhERE1IRUCQMDA/EDAAkDAwP3AAkAC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AAAAAAADAAADAAPAB4AABwALAA8AABERMxEhFSERAREzEQE1IRXAAkD9wAJAwP0AAkADAAPA/oDA/oACQAGA/oABgMDAAAAAAAQAAAMAA8AHgAADAAcACwAPAAABETMRATUhFSURMxERNSEVAwDA/QACQP0AwAMAAwABgP6AAYDAwMABgP6AAYDAwAAAAwAAA8ADwAeAAAMABwAPAAATNSEVJREzESERITUhFSMRwAGA/cDAAYD+gAMAwAPAwMDAAkD9wAJAwMD9wAAAAAEAAAPAA8AHgAAHAAABESE1IRUhEQGA/oADwP6AA8ADAMDA/QAAAAADAAADwAPAB4AAAwAHAAsAABM1IRUlETMRIREzEcACQP0AwAJAwAPAwMDAAwD9AAMA/QAAAwAAAwADwAeAAAMABwAVAAABETMRIREzERMRIzUzESEVIxEzFSMRAwDA/EDAwMDAAYDAwMAFQAGA/oACQP3A/cABgMACQMD+gMD+gAAAAAAJAAADAAPAB4AAAwAHAAsADwATABcAGwAfACMAABERMxEhETMRATUzFTM1MxUlNTMVJTUzFTM1MxUlNTMVITUzFcACQMD9AMDAwP6AwP6AwMDA/QDAAkDAAwABgP6AAYD+gAGAwMDAwMDAwMDAwMDAwMDAwMAAAAADAAADAAPAB4AAAwAPABMAABERMxETESM1MxEzETMVIxETETMRwMDAwMDAwMDABUACQP3A/cABgMACQP3AwP6AAkACQP3AAAAFAAADwAPAB4AAAwAHAAsADwATAAATNTMVMzUzFSURMxEhETMRIREzEcDAwMD+gMD9wMACQMADwMDAwMDAAYD+gAMA/QADAP0AAAAAAwAAA8ACQAkAAAMABwALAAATETMRATUzFTM1MxXAwP6AwMDAA8ADwPxABIDAwMDAAAAAAAUAAAPAA8AJAAADAAcACwAPABMAABM1IRUlETMRIREzEQE1MxUzNTMVwAJA/QDAAkDA/QDAwMADwMDAwAMA/QADAP0AA8DAwMDAAAAEAAADwAPAB4AAAwAHAAsADwAAEzUhFSURMxEhETMRATUhFcACQP0AwAJAwP0AAkADwMDAwAJA/cACQP3AAkDAwAAAAAQAAAPAA8AJwAADAAcACwAPAAATNSEVJREzESERMxEBETMRwAJA/QDAAkDA/cDAA8DAwMADAP0AAwD9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AADDrAAAxEAAAMTIAADFYAAAxggAAMacAADHOAAAx9gAAMhwAADIyAAAySAAAMl4AADJ1AAAyjwAAMqoAADLDAAAy2QAAMvIAADMJAAAzMwAAM0YAADNdAAAzdwAAM4MAADOPAAAzqgAAM7sAADPRAAAz5wAAM+4AADP4AAA0AwAANBUAADQhAAA0NQAANFIAADRhAAA0eQAANI0AADSYAAA0pwAANLoAADTLAAA03AAANPMAADUBAAA1DwAANSEAADU0AABAAAAAgAANpQQ/l8PPPUACwYAAAAAANA0HEwAAAAA0DOsHP6AAkAIQAqAAAAACAACAAEAAAAAAYAAAAAAAAABgAAAAYAAAAGAAAADAAAABIAAAASAAAAEgAAABIAAAAGAAAADwAAAA8AAAAPAAAAEgAAAAYAAAASAAAABgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAABgAAAAYAAAAPAAAAEgAAAA8AAAASAAAAFQAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAADAAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAADAAAABIAAAAMAAAAEgAAABIAAAAJAAAAEgAAABIAAAASAAAAEgAAABIAAAAPAAAAEgAAABIAAAAGAAAAEgAAAA8AAAAJAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAAAwAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAPAAAABgAAAA8AAAAVAAAABgAAAA8AAAASAAAAEgAAABIAAAAGAAAAEgAAAA8AAwAYAAAADAAAAA8AAAAPAAAAGAAAABIAAAAMAAAAEgAAAAwAAAAMAAAACQAAABIAAAASAAAABgAAAAwAAwAMAAAADAAAAA8AAAAbAAAAGwAAABsAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAHgAAABIAAAASAAAAEgAAABIAAAASAAAADAAAAAwAAAAMAAAADAAAABUAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAPAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAeAAAAEgAAABIAAAASAAAAEgAAABIAAAAGA/0ACQAAAAkD/QAJA/0AEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABgAAAAVAAAAFQAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABUAAAAPA/0ADwP9AAwAAAAJA/0ADAAAAAkD/QAMAAAACQAAAAwAAAAGAAAAHgAAABgAAAASAAAAFQAAABIAAAAPAAAADwAAABIAAAAJAAAAEgAAAAkAAAASAAAADAAAABIAAAAMAAAAFQAAAAwAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAYAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAeAAAAHgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAAAwAAAASAAAAEgAAABIAAAAMAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAPAAAAEgAAAA8AAAASAAAAEgAAABIAAAASAAAAEgAAABUAAAAVAAAAEgAAABIAAAASAAAADwAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAAB4AAAAeAAAAEgAAABIAAAASAAAAEgAAABIAAAAMAAAAEgAAABIAAAASAAAADAAAAAwAAAAMAAAACQAAAAkAAAAMAAAACQADAAwAAAAJAAAAEgAAAA8AAAAJAAAACQAAAAYAAAAGAAAAEgAAABgAAAAGAAAAGAAAABgAAAASAAAAGAAAABgAAAAYAAAADwP9ABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAADAAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAAAwAAAASAAAAEgAAABIAAAASAAAABgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAADwAAAAYAAAAPAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAADAAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAVAAAAEgAAABIAAAASAAAADAAAAAwAAAASAAAAHgAAAB4AAAAVAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAVAAAAEgAAABIAAAAVAAAAFQAAABgAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAAA8AAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAYAAAAEgAAABUAAAASAAAAEgAAABUAAAASAAAAFQAAAA8AAAASAAAAEgAAABIAAAASAAAAEgAAABUAAAASAAAAEgAAABIAAAAGAAAADAAAABIAAAAbAAAAGwAAABUAAAAPAAAAEgAAABIAAAASAAAAFQAAABUAAAASAAAAEgAAABIAAAASAAAAFQAAABUAAAAVAAAAEgAAABIAAAASAAAAEgAAABIAAAAVAAAAFQAAABUAAAAVAAAAEgAAABIAAAAVAAAADwAAABIAAAASAAAAFQAAABIAAAAVAAAAFQAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABUAAAAVAAAAFQAAABUAAAASAAAAEgAAABIAAAASAAAABgAAABIAAAASAAAAFQAAABUAAAASAAAAEgAAABUAAAAVAAAAFQAAABIAAAAGAAAAEgAAABIAAAAeAAAAHgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAJAAAACQAAAAkAAAAJAAAADwAAAA8AAAAPAAAADwAAAAwAAAAMAAAACQAAAAwAAAASAAAAGAAAAAwAAAASAAAADAAAABIAAAAJAAAACQAAAAwD+gAMAAAABgAAAAwAAAAMAAAADAAAAAwAAAAMAAAADAAAAAwAAAAMAAAADAAAAAwAAAAMAAAADAAAAAwAAAAMAAAADAAAAAwAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABgAAAAPAAAAGwAAABgAAAAYAAAAGAAAABUAAAAbAAAAGwAAACQAAAAbAAAAGwAAABsAAAAbAAAAGwAAABsAAAAbAAAAGwAAABsAAAAbAAAAGwAAABsAAAAYAAAAGAAAABgAAAAYAAAAGAAAABUAAwAYAAAAGAAAABgAAAAYAAAAGAAAABIAAAAbAAAAGAAAABIAAAASAAAAFQAAABIAAAAVAAAAFQAAAAwAAAAMAAAAEgAAABIAAAAPAAAADwAAABUAAAAVAAAAEgAAABUAAAAPAAAAFQAAABUAAAAPAAAADwAAABIAAAAJAAAACQAAABUAAAAYAAAAAAQAB//8AAgAAAAEAAAAAAAEAAAAOAAAAGAAgAAAAAgABAAEC1AABAAQAAAACAAAAAQAAAAEAAAAAABoBPgABAAAAAAAAABsAAAABAAAAAAABAAsAGwABAAAAAAACAAcAJgABAAAAAAADAA8ALQABAAAAAAAEAA8APAABAAAAAAAFAAsASwABAAAAAAAGABMAVgABAAAAAAAIABYAaQABAAAAAAAJAAwAfwABAAAAAAALABYAiwABAAAAAAAMABYAoQABAAAAAAANACgAtwABAAAAAAATACkA3wADAAEECQAAADYBCAADAAEECQABABYBPgADAAEECQACAA4BVAADAAEECQADAB4BYgADAAEECQAEAB4BgAADAAEECQAFABYBngADAAEECQAGACYBtAADAAEECQAIACwB2gADAAEECQAJABgCBgADAAEECQALACwCHgADAAEECQAMACwCSgADAAEECQANAFACdgADAAEECQATAFICxkNvcHlyaWdodCBBbmRyZXcgVHlsZXIgMjAxNE1pbmVjcmFmdGlhUmVndWxhck1pbmVjcmFmdGlhIDIuME1pbmVjcmFmdGlhIDIuMFZlcnNpb24gMi4wTWluZWNyYWZ0aWEtUmVndWxhcmh0dHA6Ly9hbmRyZXd0eWxlci5uZXRBbmRyZXcgVHlsZXJodHRwOi8vYW5kcmV3dHlsZXIubmV0aHR0cDovL2FuZHJld3R5bGVyLm5ldENyZWF0aXZlIENvbW1vbnMgQXR0cmlidXRpb24gU2hhcmUgQWxpa2VGaXZlIGJpZyBxdWFja2luZyB6ZXBoeXJzIGpvbHQgbXkgd2F4IGJlZABDAG8AcAB5AHIAaQBnAGgAdAAgAEEAbgBkAHIAZQB3ACAAVAB5AGwAZQByACAAMgAwADEANABNAGkAbgBlAGMAcgBhAGYAdABpAGEAUgBlAGcAdQBsAGEAcgBNAGkAbgBlAGMAcgBhAGYAdABpAGEAIAAyAC4AMABNAGkAbgBlAGMAcgBhAGYAdABpAGEAIAAyAC4AMABWAGUAcgBzAGkAbwBuACAAMgAuADAATQBpAG4AZQBjAHIAYQBmAHQAaQBhAC0AUgBlAGcAdQBsAGEAcgBoAHQAdABwADoALwAvAGEAbgBkAHIAZQB3AHQAeQBsAGUAcgAuAG4AZQB0AEEAbgBkAHIAZQB3ACAAVAB5AGwAZQByAGgAdAB0AHAAOgAvAC8AYQBuAGQAcgBlAHcAdAB5AGwAZQByAC4AbgBlAHQAaAB0AHQAcAA6AC8ALwBhAG4AZAByAGUAdwB0AHkAbABlAHIALgBuAGUAdABDAHIAZQBhAHQAaQB2AGUAIABDAG8AbQBtAG8AbgBzACAAQQB0AHQAcgBpAGIAdQB0AGkAbwBuACAAUwBoAGEAcgBlACAAQQBsAGkAawBlAEYAaQB2AGUAIABiAGkAZwAgAHEAdQBhAGMAawBpAG4AZwAgAHoAZQBwAGgAeQByAHMAIABqAG8AbAB0ACAAbQB5ACAAdwBhAHgAIABiAGUAZAAAAAAAAwAAAAMAAAAeAAEAAAAAAyYAAwABAAAFMAAAAAQDCAAAAK4AgAAGAC4AfgCsAX8BgwGPAZIBtwHrAe8B/wIbAjcCWQKSAscCywLdA3UDfgOKA4wDoQPOA9cEXwRjBHUEkwSdBKMEqwSzBLsEwgTKBM8E1QTZBOkE8R6FHp4e8yAjICYgMCAzIDYgOiBEIHEgeSCJIKkgrCCuILEgtCC6IQUhEyEXISAhIiEuIV4hmSIG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"
					local SmallFontBase64 =
						"AAEAAAAMAIAAAwBAT1MvMmSz/H0AAAFIAAAAYFZETVhoYG/3AAAGmAAABeBjbWFwel+AIwAADHgAAAUwZ2FzcP//AAEAAGP4AAAACGdseWa90hIhAAARqAAARRRoZWFk/hqSzwAAAMwAAAA2aGhlYQegBbsAAAEEAAAAJGhtdHhmdgAAAAABqAAABPBsb2Nh73HeDAAAVrwAAAJ6bWF4cAFBADMAAAEoAAAAIG5hbWX/R4pVAABZOAAABC1wb3N0fPqooAAAXWgAAAaOAAEAAAABAAArGZw2Xw889QAJA+gAAAAAzSamLgAAAADNJqljAAD/OASwAyAAAAAJAAIAAAAAAAAAAQAAAu7/BgAABRQAAABkBLAAAQAAAAAAAAAAAAAAAAAAATwAAQAAATwAMgAEAAAAAAABAAAAAAAAAAAAAAAAAAAAAAADAfMBkAAFAAACvAKKAAD/nAK8AooAAAD6ADIA+gAAAgAAAAAAAAAAAIAAAi8AAAAKAAAAAAAAAABQWVJTAEAAICEiAu7/BgAAAyAAyAAAAAUAAAAAAPoB9AAAACAAAAH0AAAAAAAAAfQAAAH0AAACWAAAAlgAAAJYAAAAyAAAAS0AAAEtAAABkAAAAZAAAAEsAAABkAAAAMgAAAJYAAAB9AAAAZAAAAH0AAAB9AAAAfQAAAH0AAAB9AAAAfQAAAH0AAAB9AAAAMgAAAEsAAABkAAAAZAAAAGQAAAB9AAAAfQAAAH0AAAB9AAAAfQAAAH0AAAB9AAAAfQAAAH0AAAB9AAAAZAAAAH0AAAB9AAAAfQAAAJYAAAB9AAAAfQAAAH0AAAB9AAAAfQAAAH0AAABkAAAAfQAAAGQAAACWAAAAfQAAAGQAAAB9AAAASwAAAJYAAABLAAAAlgAAAH0AAABLAAAAfQAAAH0AAAB9AAAAfQAAAH0AAAB9AAAAfQAAAH0AAABkAAAAfQAAAH0AAAB9AAAAlgAAAH0AAAB9AAAAfQAAAH0AAAB9AAAAfQAAAGQAAAB9AAAAZAAAAJYAAAB9AAAAZAAAAH0AAABkAAAAMgAAAGQAAAB9AAAAlgAAAH0AAABLAAAAfQAAAJYAAACWAAAAZAAAAGQAAACWAAAAyAAAAJYAAABkAAAAlgAAAH0AAACWAAAAZAAAAJYAAABLAAAASwAAAJYAAACWAAAASwAAAGQAAAB9AAAA4QAAAJYAAABkAAAAlgAAAH0AAACWAAAAZAAAAGQAAABkAAAAfQAAAH0AAAB9AAAAMgAAAH0AAAB9AAAAyAAAAH0AAACvAAAAfQAAAEsAAADIAAAAZAAAAGQAAABkAAAAZAAAAGQAAAB9AAAAlgAAAJYAAAAyAAAAfQAAAK8AAAB9AAAArwAAAH0AAAB9AAAAfQAAAGQAAAB9AAAAfQAAAH0AAAB9AAAAlgAAAH0AAACWAAAAfQAAAH0AAAB9AAAAfQAAAH0AAACWAAAAfQAAAH0AAAB9AAAAfQAAAH0AAABkAAAAfQAAAJYAAAB9AAAAlgAAAH0AAACWAAAArwAAAJYAAACvAAAAfQAAAH0AAACWAAAAfQAAAH0AAAB9AAAAfQAAAH0AAACWAAAAfQAAAJYAAAB9AAAAfQAAAH0AAAB9AAAAfQAAAJYAAAB9AAAAfQAAAH0AAAB9AAAAfQAAAGQAAAB9AAAAlgAAAH0AAACWAAAAfQAAAJYAAACvAAAAlgAAAK8AAAB9AAAAfQAAAJYAAAB9AAAAfQAAAH0AAAAyAAAAlgAAAH0AAABkAAAAZAAAAH0AAAB9AAAAfQAAAEsAAABkAAAAZAAAAH0AAAFFAAABRQAAAUUAAAB9AAAAfQAAAH0AAAB9AAAAfQAAAH0AAAB9AAAAlgAAAH0AAAB9AAAAfQAAAH0AAAB9AAAAZAAAAGQAAABkAAAAZAAAAJYAAAB9AAAAfQAAAH0AAAB9AAAAfQAAAH0AAABkAAAAlgAAAH0AAAB9AAAAfQAAAH0AAABkAAAAfQAAAH0AAAB9AAAAfQAAAH0AAAB9AAAAfQAAAH0AAACWAAAAfQAAAH0AAAB9AAAAfQAAAH0AAABkAAAAZAAAAGQAAABkAAAAlgAAAH0AAAB9AAAAfQAAAH0AAAB9AAAAfQAAAGQAAACWAAAAfQAAAH0AAAB9AAAAfQAAAGQAAAB9AAAAZAAAAJYAAACWAAAAfQAAAH0AAABkAAAAfQAAAH0AAAB9AAAAZAAAAH0AAACWAAAAMgAAAGQAAAAAAABAAEBAQEBAAwA+Aj/AAgAB//+AAkACP/+AAoACP/+AAsACf/9AAwACv/9AA0AC//9AA4ADP/9AA8ADP/9ABAADf/8ABEADv/8ABIAD//8ABMAEP/8ABQAEP/8ABUAEf/7ABYAEv/7ABcAE//7ABgAFP/7ABkAFP/7ABoAFf/6ABsAFv/6ABwAF//6AB0AGP/6AB4AGP/6AB8AGf/5ACAAGv/5ACEAG//5ACIAHP/5ACMAHP/5ACQAHf/4ACUAHv/4ACYAH//4ACcAIP/4ACgAIP/4ACkAIf/3ACoAIv/3ACsAI//3ACwAJP/3AC0AJP/3AC4AJf/2AC8AJv/2ADAAJ//2ADEAKP/2ADIAKP/2ADMAKf/1ADQAKv/1ADUAK//1ADYALP/1ADcALP/1ADgALf/0ADkALv/0ADoAL//0ADsAMP/0ADwAMP/0AD0AMf/zAD4AMv/zAD8AM//zAEAANP/zAEEANP/zAEIANf/yAEMANv/yAEQAN//yAEUAOP/yAEYAOP/yAEcAOf/xAEgAOv/xAEkAO//xAEoAPP/xAEsAPP/xAEwAPf/wAE0APv/wAE4AP//wAE8AQP/wAFAAQP/wAFEAQf/vAFIAQv/vAFMAQ//vAFQARP/vAFUARP/vAFYARf/uAFcARv/uAFgAR//uAFkASP/uAFoASP/uAFsASf/tAFwASv/tAF0AS//tAF4ATP/tAF8ATP/tAGAATf/sAGEATv/sAGIAT//sAGMAUP/sAGQAUP/sAGUAUf/rAGYAUv/rAGcAU//rAGgAVP/rAGkAVP/rAGoAVf/qAGsAVv/qAGwAV//qAG0AWP/qAG4AWP/qAG8AWf/pAHAAWv/pAHEAW//pAHIAXP/pAHMAXP/pAHQAXf/oAHUAXv/oAHYAX//oAHcAYP/oAHgAYP/oAHkAYf/nAHoAYv/nAHsAY//nAHwAZP/nAH0AZP/nAH4AZf/mAH8AZv/mAIAAZ//mAIEAaP/mAIIAaP/mAIMAaf/lAIQAav/lAIUAa//lAIYAbP/lAIcAbP/lAIgAbf/kAIkAbv/kAIoAb//kAIsAcP/kAIwAcP/kAI0Acf/jAI4Acv/jAI8Ac//jAJAAdP/jAJEAdP/jAJIAdf/iAJMAdv/iAJQAd//iAJUAeP/iAJYAeP/iAJcAef/hAJgAev/hAJkAe//hAJoAfP/hAJsAfP/hAJwAff/gAJ0Afv/gAJ4Af//gAJ8AgP/gAKAAgP/gAKEAgf/fAKIAgv/fAKMAg//fAKQAhP/fAKUAhP/fAKYAhf/eAKcAhv/eAKgAh//eAKkAiP/eAKoAiP/eAKsAif/dAKwAiv/dAK0Ai//d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ATkBOgE7ATwBPQE+AT8BQAFBAUIBQwFEAUUBRgFHAUgBSQFKAUsBTAFNAU4BTwFQAVEBUgFTAVQBVQFWAVcBWAFZAVoBWwFcAV0BXgFfAWABYQFiAWMBZAFlAWYAowCEAIUAlgCOAJ0A8gDzAI0A3gDxAJ4A9QD0APYAogCtAMkAxwCuAGIAYwCQAGQAywBlAMgAygDPAMwAzQDOAOkAZgDTANAA0QCvAGcA8ACRANYA1ADVAGgA6wDtAIkAagBpAGsAbQBsAG4AoABvAHEAcAByAHMAdQB0AHYAdwDqAHgAegB5AHsAfQB8ALgAoQB/AH4AgACBAOwA7gC6ALAAsQDkAOUAuwDmAOcApgDYANkABgAEAAUFLm51bGwJYWZpaTEwMDUxCWFmaWkxMDA1MglhZmlpMTAxMDAERXVybwlhZmlpMTAwNTgJYWZpaTEwMDU5CWFmaWkxMDA2MQlhZmlpMTAwNjAJYWZpaTEwMTQ1CWFmaWkxMDA5OQlhZmlpMTAxMDYJYWZpaTEwMTA3CWFmaWkxMDEwOQlhZmlpMTAxMDgJYWZpaTEwMTkzCWFmaWkxMDA2MglhZmlpMTAxMTAJYWZpaTEwMDU3CWFmaWkxMDA1MAlhZmlpMTAwMjMJYWZpaTEwMDUzB3VuaTAwQUQJYWZpaTEwMDU2CWFmaWkxMDA1NQlhZmlpMTAxMDMJYWZpaTEwMDk4DnBlcmlvZGNlbnRlcmVkCWFmaWkxMDA3MQlhZmlpNjEzNTIJYWZpaTEwMTAxCWFmaWkxMDEwNQlhZmlpMTAwNTQJYWZpaTEwMTAyCWFmaWkxMDEwNAlhZmlpMTAwMTcJYWZpaTEwMDE4CWFmaWkxMDAxOQlhZmlpMTAwMjAJYWZpaTEwMDIxCWFmaWkxMDAyMglhZmlpMTAwMjQJYWZpaTEwMDI1CWFmaWkxMDAyNglhZmlpMTAwMjcJYWZpaTEwMDI4CWFmaWkxMDAyOQlhZmlpMTAwMzAJYWZpaTEwMDMxCWFmaWkxMDAzMglhZmlpMTAwMzMJYWZpaTEwMDM0CWFmaWkxMDAzNQlhZmlpMTAwMzYJYWZpaTEwMDM3CWFmaWkxMDAzOAlhZmlpMTAwMzkJYWZpaTEwMDQwCWFmaWkxMDA0MQlhZmlpMTAwNDIJYWZpaTEwMDQzCWFmaWkxMDA0NAlhZmlpMTAwNDUJYWZpaTEwMDQ2CWFmaWkxMDA0NwlhZmlpMTAwNDgJYWZpaTEwMDQ5CWFmaWkxMDA2NQlhZmlpMTAwNjYJYWZpaTEwMDY3CWFmaWkxMDA2OAlhZmlpMTAwNjkJYWZpaTEwMDcwCWFmaWkxMDA3MglhZmlpMTAwNzMJYWZpaTEwMDc0CWFmaWkxMDA3NQlhZmlpMTAwNzYJYWZpaTEwMDc3CWFmaWkxMDA3OAlhZmlpMTAwNzkJYWZpaTEwMDgwCWFmaWkxMDA4MQlhZmlpMTAwODIJYWZpaTEwMDgzCWFmaWkxMDA4NAlhZmlpMTAwODUJYWZpaTEwMDg2CWFmaWkxMDA4NwlhZmlpMTAwODgJYWZpaTEwMDg5CWFmaWkxMDA5MAlhZmlpMTAwOTEJYWZpaTEwMDkyCWFmaWkxMDA5MwlhZmlpMTAwOTQJYWZpaTEwMDk1CWFmaWkxMDA5NglhZmlpMTAwOTcNYWZpaTEwMDQ1LjAwMQ1hZmlpMTAwNDcuMDAxAAAAAAAB//8AAA=="

					Visuals2.Fonts["SmallestPixel"][1] = RegisterFont(
						"SmallestPixel7",
						400,
						"normal",
						{ Id = "smallest-pixel7.ttf", Font = crypt.base64.decode(SmallFontBase64) }
					)

					Visuals2.Fonts["Minecraftia"][1] = RegisterFont(
						"Minecraft",
						400,
						"normal",
						{ Id = "Minecraft.ttf", Font = crypt.base64.decode(MinecraftBase64) }
					)

					--Fonts.SmallestPixel = SmallFont
					--Fonts.Pixel = Minecraft
				end
				--
				do -- Menu
					LPH_JIT_MAX(function()
						do -- Objects
							function Menu.Objects:Create(Type, Properties, Ignore)
								local Self = self
								local Object = Instance.new(Type)
								--
								if Type == "ScreenGui" and Ignore then
									Utils.Screens[Object] = true
								end
								--
								Self:Update(Object, Properties)
								--
								Utils.Instances[Object] = true
								--
								return Object
							end
							--
							function Menu.Objects:Update(Object, Properties)
								for Property, Value in next, Properties do
									if string.find(string.lower(Property), "color") and typeof(Value) == "string" then
										local Found = Menu.Themes[Value]
										--
										if Found then
											if Menu.Storage[Object] then
												Menu.Storage[Object][Property] = Value
											else
												Menu.Storage[Object] = {
													[Property] = Value,
												}
											end
											--
											Object[Property] = Found
										else
											warn("Invalid Color Type", Property, Value)
										end
									else
										if string.find(string.lower(Property), "translatable") then
											if Value then
												Menu.Labels[#Menu.Labels + 1] = Object
											end
										else
											Object[Property] = Value
										end
									end
								end
							end
							--
							function Menu.Objects:Button(Parent, Index)
								return Menu.Objects:Create("TextButton", {
									BackgroundTransparency = 1,
									Parent = Parent,
									Position = UDim2.new(0, 0, 0, 0),
									Size = UDim2.new(1, 0, 1, 0),
									Text = "",
									ZIndex = Index,
								})
							end
							--
							function Menu.Objects:Frame(
								AnchorPoint,
								Color,
								Transparency,
								BorderColor,
								BorderMode,
								BorderSize,
								Parent,
								Position,
								Size,
								Visible,
								Index
							)
								--
								return Menu.Objects:Create("Frame", {
									AnchorPoint = AnchorPoint,
									BackgroundColor3 = Color,
									BackgroundTransparency = Transparency,
									BorderColor3 = BorderColor,
									BorderMode = BorderMode,
									BorderSizePixel = BorderSize,
									Parent = Parent,
									Position = Position,
									Size = Size,
									Visible = Visible,
									ZIndex = Index,
								})
							end
							--
							function Menu.Objects:Holder(AnchorPoint, Parent, Position, Size, Visible, Index)
								return Menu.Objects:Frame(
									AnchorPoint,
									nil,
									1,
									nil,
									nil,
									0,
									Parent,
									Position,
									Size,
									Visible,
									Index
								)
							end
							--
							function Menu.Objects:HolderScroll(
								AnchorPoint,
								Parent,
								Position,
								Size,
								Visible,
								Index,
								ScrollingDirection
							)
								local Objects = {}
								--
								do -- Objects
									Objects["Content"] = Menu.Objects:Create("ScrollingFrame", {
										AnchorPoint = AnchorPoint,
										BackgroundTransparency = 1,
										BorderSizePixel = 0,
										Parent = Parent,
										Position = Position,
										Size = Size,
										ZIndex = Index,
										ClipsDescendants = true,
										AutomaticCanvasSize = (ScrollingDirection or "Y"),
										BottomImage = "rbxassetid://7783554086",
										CanvasSize = UDim2.new(0, 0, 0, 0),
										HorizontalScrollBarInset = "None",
										ElasticBehavior = "Never",
										HorizontalScrollBarInset = "None",
										MidImage = "rbxassetid://7783554086",
										ScrollBarImageColor3 = "Accent",
										ScrollBarImageTransparency = 0,
										ScrollBarThickness = 1.5,
										ScrollingDirection = (ScrollingDirection or "Y"),
										ScrollingEnabled = true,
										TopImage = "rbxassetid://7783554086",
										VerticalScrollBarInset = "None",
									})
									--
									Objects["List"] =
										Menu.Objects:List(UDim.new(0, -1), Objects["Content"], "Horizontal", "Left", "Top")
									Menu.Objects:Padding(Objects["Content"], nil, nil)
								end
								--
								return Objects["Content"]
							end
							--
							function Menu.Objects:Outline(
								AnchorPoint,
								Parent,
								Position,
								Size,
								Frame,
								Inline,
								Outline,
								Index
							)
								local Outline = Menu.Objects:Frame(
									AnchorPoint,
									(Inline or "Light Inline"),
									nil,
									(Outline or "Outline"),
									"Inset",
									1,
									Parent,
									Position,
									Size,
									nil,
									Index
								)
								--
								return Outline,
									(Frame and Menu.Objects:Frame(
										nil,
										Frame,
										nil,
										nil,
										nil,
										0,
										Outline,
										UDim2.new(0, 1, 0, 1),
										UDim2.new(1, -2, 1, -2),
										nil,
										Index
									))
							end
							--
							function Menu.Objects:Text(
								Translatable,
								AnchorPoint,
								Parent,
								Position,
								Size,
								Name,
								Color,
								Alignment,
								Index,
								Truncate,
								TextSize,
								S_Font
							)
								return Menu.Objects:Create("TextLabel", {
									AnchorPoint = AnchorPoint,
									BackgroundTransparency = 1,
									BorderSizePixel = 0,
									Parent = Parent,
									Position = (Position or UDim2.new(0, 0, 0, -1)),
									Size = Size,
									FontFace = Visuals2.Fonts["SmallestPixel"][1],
									Text = Name,
									TextColor3 = (Color or "Light Text"),
									TextSize = (TextSize or Visuals2.Fonts["SmallestPixel"][2]),
									TextXAlignment = (Alignment or "Left"),
									ZIndex = Index,
									TextTruncate = Truncate,
									Translatable = Translatable or nil,
									RichText = true,
								})
							end
							--
							function Menu.Objects:Box(
								AnchorPoint,
								Parent,
								Position,
								Size,
								Name,
								Placeholder,
								Color,
								Alignment,
								Index,
								Truncate
							)
								return Menu.Objects:Create("TextBox", {
									AnchorPoint = AnchorPoint,
									BackgroundTransparency = 1,
									BorderSizePixel = 0,
									Parent = Parent,
									Position = (Position or UDim2.new(0, 0, 0, -1)),
									Size = Size,
									FontFace = Visuals2.Fonts["SmallestPixel"][1],
									PlaceholderColor3 = "Dark Text",
									PlaceholderText = Placeholder,
									Text = Name,
									TextColor3 = (Color or "Light Text"),
									TextSize = Visuals2.Fonts["SmallestPixel"][2],
									TextXAlignment = (Alignment or "Center"),
									ZIndex = Index,
									TextTruncate = Truncate,
									ClearTextOnFocus = false,
								})
							end
							--
							function Menu.Objects:Scroll(
								Parent,
								Gradients,
								Offset,
								Bottom,
								Top,
								Size,
								Index,
								ScrollingDirection
							)
								local Objects = {}
								--
								do -- Objects
									Objects["Bar"] = Menu.Objects:Frame(
										Vector2.new(1, 0),
										"Light Inline",
										nil,
										nil,
										nil,
										0,
										Parent,
										UDim2.new(1, 1, 0, 0),
										UDim2.new(0, ((Size or 2) + 2), 1, 0),
										false,
										Index
									)
									Objects["Content"] = Menu.Objects:Create("ScrollingFrame", {
										BackgroundTransparency = 1,
										BorderSizePixel = 0,
										Parent = Parent,
										Position = UDim2.new(0, 0, 0, 1),
										Size = UDim2.new(1, 0, 1, -2),
										ZIndex = Index,
										ClipsDescendants = true,
										AutomaticCanvasSize = (ScrollingDirection or "Y"),
										BottomImage = "rbxassetid://7783554086",
										CanvasSize = UDim2.new(0, 0, 0, 0),
										HorizontalScrollBarInset = "None",
										ElasticBehavior = "Never",
										HorizontalScrollBarInset = "None",
										MidImage = "rbxassetid://7783554086",
										ScrollBarImageColor3 = "Accent",
										ScrollBarImageTransparency = 0,
										ScrollBarThickness = (Size or 2),
										ScrollingDirection = (ScrollingDirection or "Y"),
										ScrollingEnabled = true,
										TopImage = "rbxassetid://7783554086",
										VerticalScrollBarInset = "None",
									})
									--
									Objects["List"] =
										Menu.Objects:List(Offset, Objects["Content"], "Vertical", "Center", "Top")
									--
									Menu.Objects:Padding(Objects["Content"], Bottom, Top)
									--
									if Gradients then
										for Index2, Value in
											next,
											{ "rbxassetid://14557257303", "rbxassetid://14557262853" }
										do
											local Gradient = Menu.Objects:Create("ImageLabel", {
												BackgroundTransparency = 1,
												BorderSizePixel = 0,
												Parent = Parent,
												Position = UDim2.new(
													0,
													0,
													(Index2 == 2 and 1 or 0),
													(Index2 == 2 and -22 or 0)
												),
												Size = UDim2.new(1, -5, 0, 22),
												Image = Value,
												ImageColor3 = "Light Background",
												ZIndex = Index,
											})
										end
									end
								end
								--
								local SizeConnection, ContentSizeConnection
								--
								do -- Connections
									SizeConnection = Utils:Connect(
										Objects["Content"]:GetPropertyChangedSignal("AbsoluteSize"),
										function()
											Objects["Bar"].Visible = (
												(
													Objects["Content"].AbsoluteCanvasSize.Y
													- Objects["Content"].AbsoluteWindowSize.Y
												) > 0
											)
										end
									)
									--
									ContentSizeConnection = Utils:Connect(
										Objects["List"]:GetPropertyChangedSignal("AbsoluteContentSize"),
										function()
											Objects["Bar"].Visible = (
												(
													Objects["Content"].AbsoluteCanvasSize.Y
													- Objects["Content"].AbsoluteWindowSize.Y
												) > 0
											)
										end
									)
								end
								--
								return Objects["Content"], SizeConnection, ContentSizeConnection
							end
							--
							function Menu.Objects:List(Padding, Parent, Direction, Horizontal, Vertical)
								return Menu.Objects:Create("UIListLayout", {
									Padding = Padding,
									Parent = Parent,
									FillDirection = Direction,
									HorizontalAlignment = Horizontal,
									VerticalAlignment = Vertical,
								})
							end
							--
							function Menu.Objects:Stroke(StrokeMode, Color, JoinMode, Thickness, Transparency, Parent)
								return Menu.Objects:Create("UIStroke", {
									ApplyStrokeMode = StrokeMode,
									Color = Color,
									LineJoinMode = JoinMode,
									Thickness = Thickness,
									Transparency = Transparency,
									Parent = Parent,
								})
							end
							--
							function Menu.Objects:Padding(Parent, Bottom, Top, Left, Right)
								return Menu.Objects:Create("UIPadding", {
									Parent = Parent,
									PaddingBottom = Bottom,
									PaddingLeft = Left,
									PaddingRight = Right,
									PaddingTop = Top,
								})
							end
							--
							function Menu.Objects:Gradient(Color, Rotation, Transparency, Parent)
								return Menu.Objects:Create("UIGradient", {
									Color = (Color and ColorSequence.new({
										ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 255, 255)),
										ColorSequenceKeypoint.new(1, Color),
									})),
									Rotation = Rotation,
									Transparency = (Transparency and NumberSequence.new({
										NumberSequenceKeypoint.new(0, Transparency),
										NumberSequenceKeypoint.new(1, Transparency),
									})),
									Parent = Parent,
								})
							end
						end
						--
						do -- Functions
							local Screen = Menu.Objects:Create("ScreenGui", {
								DisplayOrder = 9999,
								Enabled = true,
								IgnoreGuiInset = true,
								Parent = gethui(),
								ResetOnSpawn = false,
							})
							--
							do -- Init
								for Theme, Color in next, Menu.Presets.Themes.Niggahack do
									Menu.Themes[Theme] = Color
								end
							end
							--
							do -- Main
								function Menu:Create(Properties)
									local Properties = (Properties or {})
									local Window = {
										Objects = {},
										Content = {},
										Opened = {
											Dropdown = {},
											Picker = {},
											Bind = {},
											BindMode = {},
											CopyAndPaste = {},
										},
										Pages = {},
										Holds = {},
										Binds = {},
										Particles = {},
										Lines = {},
										CopiedColor = nil,
										--
										Inset = Services.GuiService:GetGuiInset(),
										State = true,
										Init = false,
										--
										Self = (self.Content and self),
										--
										Stamp = (Utils:Property(Properties, "Stamp", "Description") or ""),
										Name = (Utils:Property(Properties, "Name", "Text") or "Window"),
										Size = (Utils:Property(Properties, "Size") or Vector2.new(552, 451)),
										Single = (Utils:Property(Properties, "Combined", "Combine", "Single") or false),
										Scrollable = (Utils:Property(Properties, "Scrollable", "Scroll") or false),
									}
									--
									do -- Objects
										Window.Objects["Screen"] = Screen
										--
										Window.Objects["Screen_Outline"], Window.Objects["Screen_Frame"] =
											Menu.Objects:Outline(
												nil,
												Window.Objects["Screen"],
												UDim2.new(
													(Window.Self and 1 or 0),
													(Window.Self and 11 or 100),
													0,
													(Window.Self and -1 or 100)
												),
												UDim2.new(0, Window.Size.X, 0, Window.Size.Y),
												"Dark Background"
											)
										--
										Window.Objects["Screen_Glow"] = Menu.Objects:Create("ImageLabel", {
											BackgroundTransparency = 1,
											BorderSizePixel = 0,
											Parent = Window.Objects["Screen_Outline"],
											Position = UDim2.new(0.5, 0, 0.5, 0),
											Size = UDim2.new(1, 48, 1, 48),
											ImageColor3 = Color3.fromRGB(80, 80, 80),
											ScaleType = "Slice",
											ZIndex = -1,
											SliceCenter = Rect.new(49, 49, 450, 450),
											ImageTransparency = 0.6,
											AnchorPoint = Vector2.new(0.5, 0.5),
											ImageColor3 = "Accent",
											Image = "rbxassetid://6015897843",
										})
										--
										Menu.Objects:Gradient(
											Color3.fromRGB(0, 0, 0),
											-90,
											0.4,
											Window.Objects["Screen_Frame"]
										)
										--
										Window.Objects["Screen_Resizer"] = Menu.Objects:Create("ImageButton", {
											BackgroundColor3 = Color3.fromRGB(0, 0, 0),
											BackgroundTransparency = 1,
											BorderColor3 = Color3.fromRGB(0, 0, 0),
											BorderSizePixel = 0,
											Parent = Window.Objects["Screen_Outline"],
											Position = UDim2.new(1, -9, 1, -9),
											Size = UDim2.new(0, 10, 0, 10),
											ImageColor3 = Color3.fromRGB(80, 80, 80),
											ScaleType = "Fit",
											ZIndex = 5,
											ImageTransparency = 0,
										})
										--
										Window.Objects["Tooltip_Outline"], Window.Objects["Tooltip_Frame"] =
											Menu.Objects:Outline(
												nil,
												Window.Objects["Screen"],
												UDim2.new(2),
												nil,
												"Lighter Background"
											)
										--
										Window.Objects["ToolTip_Text"] = Menu.Objects:Text(
											true,
											nil,
											Window.Objects["Tooltip_Frame"],
											UDim2.new(0, 3),
											UDim2.new(1, -37, 1, 0),
											"",
											"Light Text",
											"Left"
										)
										--
										do -- Top
											local Frame_Top = Menu.Objects:Holder(
												Vector2.new(0, 1),
												Window.Objects["Screen_Frame"],
												UDim2.new(0, 10, 0, 26),
												UDim2.new(1, -20, 0, 20)
											)
											--
											local Gradient_Line1 = Menu.Objects:Frame(
												nil,
												"Accent",
												nil,
												nil,
												nil,
												0,
												Window.Objects["Screen_Frame"],
												nil,
												UDim2.new(1, 0, 0, 1)
											)
											local Gradient_Line2 = Menu.Objects:Frame(
												nil,
												"Outline",
												0.45,
												nil,
												nil,
												0,
												Window.Objects["Screen_Frame"],
												UDim2.new(0, 0, 0, 1),
												UDim2.new(1, 0, 0, 1)
											)
											--
											if not Window.Self then
												Window.Objects["Button"] = Menu.Objects:Button(Frame_Top)
											end
										end
										--
										do -- Buttons
											if Window.Scrollable then
												Window.Objects["Buttons"] = Menu.Objects:HolderScroll(
													Vector2.new(0, 1),
													Window.Objects["Screen_Frame"],
													UDim2.new(0, 10, 0, 41),
													UDim2.new(1, -21, 0, 30),
													nil,
													nil,
													"X"
												)
											else
												Window.Objects["Buttons"] = Menu.Objects:Holder(
													Vector2.new(0, 1),
													Window.Objects["Screen_Frame"],
													UDim2.new(0, 10, 0, 41),
													UDim2.new(1, -20, 0, 30),
													nil,
													nil,
													"X"
												)
												--
												Menu.Objects:List(
													UDim.new(0, -1),
													Window.Objects["Buttons"],
													"Horizontal",
													"Left",
													"Top"
												)
											end
										end
										--
										do -- Outline
											Window.Objects["Outline"], Window.Objects["Frame"] = Menu.Objects:Outline(
												Vector2.new(0, 1),
												Window.Objects["Screen_Frame"],
												UDim2.new(0, 10, 1, -5),
												UDim2.new(
													1,
													-20
														+ (
															Window.PageAmmount ~= nil
																and Window.PageAmmount % 2 == 0
																and 1
															or 0
														),
													1,
													-47
												),
												"Dark Background"
											)
										end
									end
									--
									do -- Functions
										function Window:Initialise()
											--
											local Self = self
											local Size = (1 / #Self.Pages)
											--
											for Index, Value in next, Self.Pages do
												Value:Initialise(Size)
											end
											--
											if not Window.Self and Flags["Binds"] then
												Flags["Binds"]:Update()
											end
											--
											Self.Init = true
										end
										--
										function Window:Update(Began)
											if Window.Init then
												for Bind, Value in next, Window.Binds do
													local State = Bind.State
													--
													if #State > 0 then
														local BindInput = Enum[State[1]][State[2]]
														--
														if BindInput == self then
															Bind:Update(Began)
														end
													end
												end
											end
										end
										--
										function Window:SetLanguage(State)
											if not Menu.OriginalText then
												Menu.OriginalText = {}
												for Index, Value in pairs(Menu.Labels) do
													Menu.OriginalText[Index] = Value.Text
												end
											end
											--
											Languages.Locale = State
											--
											for Index, Value in pairs(Menu.Labels) do
												if not Menu.OriginalText[Index] then
													Menu.OriginalText[Index] = Menu.Labels[Index].Text
												end
												--
												Value.Text = Languages:Translate(Menu.OriginalText[Index])
											end
										end
										--
										function Window:Scroll(Content, Holder)
											local ContentSize = Content.AbsoluteSize
											--
											if Content.AbsoluteCanvasSize.Y > ContentSize.Y then
												local ContentPosition = (Content.AbsolutePosition + Vector2.new(0, 6))
												local HolderPosition = Holder.AbsolutePosition
												--
												if HolderPosition.Y < ContentPosition.Y then
													Content.CanvasPosition = Vector2.new(
														0,
														Content.CanvasPosition.Y
															+ math.floor((HolderPosition - ContentPosition).Y - 16)
													)
												else
													local FullHolderPosition = (HolderPosition + Holder.AbsoluteSize)
													local FullContentPosition = (
														ContentPosition + (ContentSize - Vector2.new(0, 12))
													)
													--
													if FullHolderPosition.Y > FullContentPosition.Y then
														Content.CanvasPosition = Vector2.new(
															0,
															Content.CanvasPosition.Y
																+ math.floor(
																	(FullHolderPosition - FullContentPosition).Y + 16
																)
														)
													end
												end
											end
										end
										--
										function Window:Switch(State)
											self.State = State
											--
											self.Objects["Screen_Outline"].Visible = self.State
										end
										--
										do -- Reposition
											if not Window.Self then
												function Window:Reposition(Location)
													local Size = Services.Workspace.CurrentCamera.ViewportSize
													--
													self.Objects["Screen_Outline"].Position = UDim2.new(
														0,
														(Location.X - self.Drag.X),
														0,
														(Location.Y - self.Drag.Y + self.Inset.Y)
													)
												end
											end
										end
										--
										do -- Content
											do -- Functions
												function Window:Close(Self, Type)
													local Opened = false
													--
													for Index, Value in next, self.Opened do
														for Index2, Value2 in next, Value do
															if Index2 == Self and Index == Type then
																Opened = true
															else
																Value2:Close()
															end
														end
													end
													--
													return Opened
												end
												--
												function Window:Open(Content, Type)
													local Opened = self:Close(Content, Type)
													--
													if not Opened then
														self.Content[Type](Content, Type)
													end
												end
											end
											--
											function Window.Content.Dropdown(Self, Type)
												local Content = {
													Connections = {},
													Objects = {},
													Items = {},
													--
													Tick = tick(),
												}
												--
												do -- Init
													Window:Scroll(Self.Section.Objects["Content"], Self.Objects["Holder"])
												end
												--
												do -- Objects
													local SelfPosition = (
														Self.Objects["Outline"].AbsolutePosition
														- Self.Section.Objects["Content"].AbsolutePosition
													)
													local OutlinePosition = UDim2.new(
														0,
														SelfPosition.X,
														0,
														math.floor(
															SelfPosition.Y + Self.Objects["Outline"].AbsoluteSize.Y - 1
														)
													)
													--
													Content.Objects["Outline"], Content.Objects["Frame"] =
														Menu.Objects:Outline(
															nil,
															Self.Section.Objects["Open"],
															OutlinePosition,
															UDim2.new(
																0,
																Self.Objects["Outline"].AbsoluteSize.X,
																0,
																(
																	math.clamp(
																		Self.Shown,
																		0,
																		(
																			Self.Shown
																					> Utils.Table:Count(Self.Selected)
																				and Utils.Table:Count(Self.Selected)
																			or 999
																		)
																	) * 20
																) + 6
															),
															"Lighter Background",
															nil,
															nil,
															2
														)
													--
													Menu.Objects:Gradient(
														Color3.fromRGB(0, 0, 0),
														-90,
														0.6,
														Content.Objects["Frame"]
													)
													--
													Content.Objects["Content"], Content.Connections["Size"], Content.Connections["ContentSize"] =
														Menu.Objects:Scroll(
															Content.Objects["Frame"],
															false,
															nil,
															nil,
															nil,
															3,
															2
														)
													--
													do -- Options
														local currentState = Self:Get()
														for index, value in next, Self.Selected do
															local realIndex = (typeof(value) == "table" and value[1])
																or index
															local realValue = (typeof(value) == "table" and value[2])
																or value

															local isSelected = false
															if typeof(currentState) == "table" then
																isSelected = table.find(currentState, realValue) ~= nil
															else
																isSelected = (currentState == realValue)
															end

															local Item = {
																Objects = {},
																State = isSelected,
															}

															Item.Objects["Holder"] = Menu.Objects:Holder(
																nil,
																Content.Objects["Content"],
																nil,
																UDim2.new(1, 0, 0, 20),
																nil,
																2
															)
															Item.Objects["Text"] = Menu.Objects:Text(
																false,
																nil,
																Item.Objects["Holder"],
																UDim2.new(0, 8, 0, 0),
																UDim2.new(1, -14, 1, 0),
																realValue,
																nil,
																nil,
																2
															)
															Item.Objects["Button"] =
																Menu.Objects:Button(Item.Objects["Holder"], 2)

															function Item:Set(selected)
																Item.State = selected
																Menu.Objects:Update(Item.Objects["Text"], {
																	Position = UDim2.new(0, (selected and 9 or 8), 0, 0),
																	Size = UDim2.new(1, -(selected and 16 or 14), 1, 0),
																	TextColor3 = (selected and "Accent" or "Light Text"),
																})
															end

															if isSelected then
																Item:Set(true)
															end

															Content.Connections[#Content.Connections + 1] = Utils:Connect(
																Item.Objects["Button"].MouseButton1Down,
																function()
																	if Self.Maximum == 1 then
																		if Self:Get() ~= realValue then
																			Self:Set(realValue)
																			for idx2, item2 in pairs(Content.Items) do
																				local selValue = (
																					typeof(Self.Selected[idx2])
																						== "table"
																					and Self.Selected[idx2][2]
																				)
																					or Self.Selected[idx2]
																				item2:Set(realValue == selValue)
																			end
																		end
																	else
																		local current = Self:Get()
																		local newState = {}

																		if typeof(current) ~= "table" then
																			current = { current }
																		end

																		for i, v in ipairs(current) do
																			newState[i] = v
																		end

																		local pos = table.find(newState, realValue)
																		if pos then
																			table.remove(newState, pos)
																		else
																			if #newState < Self.Maximum then
																				table.insert(newState, realValue)
																			end
																		end

																		if #newState < Self.Minimum then
																			newState = current
																		end

																		Self:Set(newState)

																		for idx2, item2 in pairs(Content.Items) do
																			local selValue = (
																				typeof(Self.Selected[idx2]) == "table"
																				and Self.Selected[idx2][2]
																			)
																				or Self.Selected[idx2]
																			if typeof(newState) == "table" then
																				item2:Set(
																					table.find(newState, selValue) ~= nil
																				)
																			else
																				item2:Set(newState == selValue)
																			end
																		end
																	end
																end
															)

															Content.Items[realIndex] = Item
														end
													end
												end
												--
												do -- Functions
													function Content:Close()
														for Index, Value in next, Content.Connections do
															Value:Disconnect()
														end
														--
														if Content.Objects["Outline"] then
															Content.Objects["Outline"]:Remove()
														end
														--
														Self.Objects["Arrow"].Rotation = 0
														--
														Content.Connections = nil
														Content.Objects = nil
														--
														for Index, Value in next, Content.Items do
															Value.Objects = nil
															Value.State = nil
															--
															Value = nil
														end
														--
														Content.Items = nil
														Content.Init = nil
														Content.Tick = nil
														--
														Content = nil
														--
														Window.Opened[Type][Self] = nil
													end
													--
													function Content:Input(Mouse)
														if Content.Init or ((tick() - Content.Tick) >= 0.05) then
															local Location = (Utils:Location() - Window.Inset)
															--
															local ContentPosition =
																Content.Objects["Content"].AbsolutePosition
															local ContentSize = Content.Objects["Content"].AbsoluteSize
															--
															if
																not Utils.Math:Over(Location, ContentPosition, ContentSize)
															then
																local Extra = Self.Objects["Extra"]
																--
																if Mouse and Extra then
																	local ExtraPosition = Extra.AbsolutePosition
																	local ExtraSize = Extra.AbsoluteSize
																	--
																	if
																		not Utils.Math:Over(
																			Location,
																			ExtraPosition,
																			ExtraSize
																		)
																	then
																		Content:Close()
																	end
																else
																	Content:Close()
																end
															end
														end
														--
														if Content then
															Content.Init = true
														end
													end
												end
												--
												do -- Connections
													Content.Connections[#Content.Connections + 1] = Utils:Connect(
														Services.UserInputService.InputBegan,
														function(Input)
															if Input.UserInputType == Enum.UserInputType.MouseButton1 then
																Content:Input(true)
															end
														end
													)
													--
													Content.Connections[#Content.Connections + 1] = Utils:Connect(
														Services.UserInputService.InputChanged,
														function(Input)
															if Input.UserInputType == Enum.UserInputType.MouseWheel then
																Content:Input()
															end
														end
													)
												end
												--
												do -- Setup
													Self.Objects["Arrow"].Rotation = 180
													--
													Window.Opened[Type][Self] = Content
												end
											end
											--
											function Window.Content.Picker(Self, Type)
												local Content = {
													Connections = {},
													Objects = {},
													Buttons = {},
													Items = {},
													--
													Tick = tick(),
												}
												--
												do -- Init
													Window:Scroll(Self.Section.Objects["Content"], Self.Objects["Holder"])
												end
												--
												do -- Objects
													local SelfPosition = (
														Self.Objects["Outline"].AbsolutePosition
														- Self.Section.Objects["Content"].AbsolutePosition
													)
													local OutlinePosition = UDim2.new(
														0,
														SelfPosition.X,
														0,
														math.floor(
															SelfPosition.Y + Self.Objects["Outline"].AbsoluteSize.Y + 4
														)
													)
													local OutlineSize = Vector2.new((Self.Alpha and 200 or (200 - 16)), 165)
													--
													Content.Objects["Outline"], Content.Objects["Frame"] =
														Menu.Objects:Outline(
															nil,
															Self.Section.Objects["Open"],
															OutlinePosition,
															UDim2.new(0, OutlineSize.X, 0, OutlineSize.Y),
															"Lighter Background",
															nil,
															nil,
															2
														)
													--
													local SaturationSize = (
														OutlineSize.X
														- 14
														- ((13 + 3) * (Self.Alpha and 2 or 1))
													)
													--
													for Index, Value in
														next,
														{ "Saturation", "Hue", (Self.Alpha and "Alpha") }
													do
														local BoxPosition = UDim2.new(
															0,
															(
																Value == "Saturation" and 5
																or Value == "Hue" and (SaturationSize + 5 + 3)
																or (SaturationSize + 5 + 13 + 6)
															),
															0,
															5
														)
														local BoxSize = UDim2.new(
															0,
															(Value == "Saturation" and SaturationSize or 13),
															0,
															(OutlineSize.Y - 14)
														)
														--
														Content.Objects[Value .. "_Outline"] = Menu.Objects:Outline(
															nil,
															Content.Objects["Frame"],
															BoxPosition,
															BoxSize,
															nil,
															Color3.fromHSV(
																Self.State[1],
																(Value == "Alpha" and Self.State[2] or 1),
																1
															),
															nil,
															2
														)
														--
														if Value == "Hue" then
															Content.Objects[Value .. "_Image"] =
																Menu.Objects:Create("Frame", {
																	BackgroundTransparency = 0,
																	BorderSizePixel = 0,
																	Parent = Content.Objects[Value .. "_Outline"],
																	Position = UDim2.new(0, 0, 0, 0),
																	Size = UDim2.new(1, 0, 1, 0),
																	ZIndex = 2,
																	BackgroundColor3 = Color3.fromRGB(255, 255, 255),
																})
															--
															Menu.Objects:Create("UIGradient", {
																Color = ColorSequence.new({
																	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)),
																	ColorSequenceKeypoint.new(
																		0.17,
																		Color3.fromRGB(255, 0, 255)
																	),
																	ColorSequenceKeypoint.new(
																		0.33,
																		Color3.fromRGB(0, 0, 255)
																	),
																	ColorSequenceKeypoint.new(
																		0.5,
																		Color3.fromRGB(0, 255, 255)
																	),
																	ColorSequenceKeypoint.new(
																		0.67,
																		Color3.fromRGB(0, 255, 0)
																	),
																	ColorSequenceKeypoint.new(
																		0.83,
																		Color3.fromRGB(255, 255, 0)
																	),
																	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 0, 0)),
																}),
																Rotation = 270, -- 270
																Parent = Content.Objects[Value .. "_Image"],
															})
														else
															Content.Objects[Value .. "_Image"] =
																Menu.Objects:Create("ImageLabel", {
																	BackgroundTransparency = 1,
																	BorderSizePixel = 0,
																	Parent = Content.Objects[Value .. "_Outline"],
																	Position = UDim2.new(0, 0, 0, 0),
																	Size = UDim2.new(1, 0, 1, 0),
																	Image = Menu.Images[Value],
																	ZIndex = 2,
																})
														end
														--
														Content.Objects[Value .. "_Cursor"] = Menu.Objects:Frame(
															Vector2.new((Value == "Saturation" and 0.5 or 0), 0.5),
															"Outline",
															nil,
															nil,
															nil,
															0,
															Content.Objects[Value .. "_Image"],
															nil,
															(
																Value == "Saturation" and UDim2.new(0, 4, 0, 4)
																or UDim2.new(1, 2, 0, 4)
															),
															nil,
															2
														)
														Content.Objects[Value .. "_Cursor2"] = Menu.Objects:Frame(
															nil,
															nil,
															nil,
															nil,
															nil,
															0,
															Content.Objects[Value .. "_Cursor"],
															UDim2.new(0, 1, 0, 1),
															UDim2.new(1, -2, 1, -2),
															nil,
															2
														)
														--
														Content.Buttons[Value] =
															Menu.Objects:Button(Content.Objects[Value .. "_Outline"], 2)
													end
												end
												--
												do -- Functions
													function Content:Close()
														for Index, Value in next, Content.Connections do
															Value:Disconnect()
														end
														--
														if Content.Objects["Outline"] then
															Content.Objects["Outline"]:Remove()
														end
														--
														Content.Connections = nil
														Content.Objects = nil
														Content.Buttons = nil
														Content.Items = nil
														--
														Content.Init = nil
														Content.Tick = nil
														--
														Content = nil
														--
														Window.Opened[Type][Self] = nil
													end
													--
													function Content:Update(Type)
														local Size =
															Content.Objects[(Type == "Saturation" and "Saturation_Image" or Type == "Hue" and "Hue_Image" or Type == "Alpha" and "Alpha_Image")].AbsoluteSize
														--
														if Type == "Saturation" then
															Content.Objects["Saturation_Cursor"].Position = UDim2.new(
																0,
																math.clamp((Self.State[2] * Size.X), 1, Size.X - 1),
																0,
																math.clamp(((1 - Self.State[3]) * Size.Y), 1, Size.Y - 1)
															)
															Content.Objects["Saturation_Cursor2"].BackgroundColor3 =
																Color3.fromRGB(0, 0, 0)
														elseif Type == "Hue" then
															Content.Objects["Hue_Cursor"].Position = UDim2.new(
																0,
																-1,
																0,
																math.clamp((Self.State[1] * Size.Y), 1, Size.Y - 1)
															)
															--
															Content.Objects["Saturation_Outline"].BackgroundColor3 =
																Color3.fromHSV(Self.State[1], 1, 1)
															Content.Objects["Hue_Cursor2"].BackgroundColor3 =
																Color3.fromRGB(0, 0, 0)
															Content.Objects["Saturation_Cursor2"].BackgroundColor3 =
																Color3.fromRGB(0, 0, 0)
														elseif Type == "Alpha" then
															Content.Objects["Alpha_Cursor"].Position = UDim2.new(
																0,
																-1,
																0,
																math.clamp((Self.State[4] * Size.Y), 1, Size.Y - 1)
															)
														end
														--
														if Content.Objects["Alpha_Outline"] then
															Content.Objects["Alpha_Cursor2"].BackgroundColor3 =
																Color3.fromRGB(0, 0, 0)
															Content.Objects["Alpha_Outline"].BackgroundColor3 =
																Color3.fromHSV(Self.State[1], Self.State[2], 1)
														end
													end
													--
													function Content:Refresh()
														local Location = Utils:Location()
														--
														if Content.Saturation then
															Self.State[2] = (
																Utils.Math:Value(
																	Location.X,
																	Content.Objects["Saturation_Image"].AbsolutePosition.X
																		+ Window.Inset.X,
																	Content.Objects["Saturation_Image"].AbsoluteSize.X
																)
															)
															Self.State[3] = (
																1
																- Utils.Math:Value(
																	Location.Y,
																	Content.Objects["Saturation_Image"].AbsolutePosition.Y
																		+ Window.Inset.Y,
																	Content.Objects["Saturation_Image"].AbsoluteSize.Y
																)
															)
															--
															Content:Update("Saturation")
															--
															Self:Set(Self.State)
														elseif Content.Hue then
															Self.State[1] = (
																Utils.Math:Value(
																	Location.Y,
																	Content.Objects["Hue_Image"].AbsolutePosition.Y
																		+ Window.Inset.Y,
																	Content.Objects["Hue_Image"].AbsoluteSize.Y
																)
															)
															--
															Content:Update("Hue")
															--
															Self:Set(Self.State)
														elseif Content.Alpha then
															Self.State[4] = (
																Utils.Math:Value(
																	Location.Y,
																	Content.Objects["Alpha_Image"].AbsolutePosition.Y
																		+ Window.Inset.Y,
																	Content.Objects["Alpha_Image"].AbsoluteSize.Y
																)
															)
															--
															Content:Update("Alpha")
															--
															Self:Set(Self.State)
														end
													end
													--
													function Content:Input(Mouse)
														if Content.Init or ((tick() - Content.Tick) >= 0.05) then
															local Location = (Utils:Location() - Window.Inset)
															--
															for Index, Value in next, Content.Buttons do
																local ButtonPosition = Value.AbsolutePosition
																local ButtonSize = Value.AbsoluteSize
																--
																if
																	Utils.Math:Over(Location, ButtonPosition, ButtonSize)
																then
																	return
																end
															end
															--
															Content:Close()
														end
														--
														if Content then
															Content.Init = true
														end
													end
												end
												--
												do -- Connections
													for Index, Value in next, Content.Buttons do
														Content.Connections[#Content.Connections + 1] = Utils:Connect(
															Value.MouseButton1Down,
															function(Input)
																Content[Index] = true
																--
																Window.Holds[Content] = Index
																--
																Content:Refresh()
															end
														)
													end
													--
													Content.Connections[#Content.Connections + 1] = Utils:Connect(
														Services.UserInputService.InputBegan,
														function(Input)
															if Input.UserInputType == Enum.UserInputType.MouseButton1 then
																Content:Input(true)
															end
														end
													)
													--
													Content.Connections[#Content.Connections + 1] = Utils:Connect(
														Services.UserInputService.InputChanged,
														function(Input)
															if Input.UserInputType == Enum.UserInputType.MouseWheel then
																Content:Close()
															end
														end
													)
												end
												--
												do -- Setup
													Content:Update("Saturation")
													Content:Update("Hue")
													if Self.Alpha then
														Content:Update("Alpha")
													end
													--
													Window.Opened[Type][Self] = Content
												end
											end
											--
											function Window.Content.Bind(Self, Type)
												local Content = {
													Connections = {},
													--
													Tick = tick(),
												}
												--
												do -- Functions
													function Content:Close()
														for Index, Value in next, Content.Connections do
															Value:Disconnect()
														end
														--
														Content.Connections = nil
														--
														Content = nil
														--
														Menu.Objects:Update(Self.Objects["Value"], {
															TextColor3 = "Dark Text",
														})
														--
														Window.Opened[Type][Self] = nil
													end
													--
													function Content:Input(Input)
														if (tick() - Content.Tick) >= 0.05 then
															local Key = (
																Input.KeyCode.Name ~= "Unknown" and Input.KeyCode
																or Input.UserInputType
															)
															--
															if Key then
																if Key.Name == "Backspace" or Key.Name == "Escape" then
																	Content:Close()
																elseif Menu.Inputs.KeyCodes[Key.Name] then
																	Self:Set(Key)
																	--
																	Content:Close()
																end
															end
														end
													end
												end
												--
												do -- Connections
													Content.Connections[#Content.Connections + 1] = Utils:Connect(
														Services.UserInputService.InputBegan,
														function(Input)
															Content:Input(Input)
														end
													)
												end
												--
												do -- Setup
													Self:Set()
													--
													Menu.Objects:Update(Self.Objects["Value"], {
														TextColor3 = "Accent",
													})
													--
													Window.Opened[Type][Self] = Content
												end
											end
											--
											function Window.Content.BindMode(Self, Type)
												local Content = {
													Connections = {},
													Objects = {},
													Items = {},
													--
													Tick = tick(),
												}
												--
												do -- Init
													Window:Scroll(Self.Section.Objects["Content"], Self.Objects["Holder"])
												end
												--
												do -- Objects
													local SelfPosition = (
														Self.Objects["Holder"].AbsolutePosition
														- Self.Section.Objects["Content"].AbsolutePosition
													)
													local OutlinePosition = UDim2.new(
														0,
														(SelfPosition.X + 1),
														0,
														math.floor(
															SelfPosition.Y + Self.Objects["Holder"].AbsoluteSize.Y + 2
														)
													)
													--
													Content.Objects["Outline"], Content.Objects["Frame"] =
														Menu.Objects:Outline(
															nil,
															Self.Section.Objects["Open"],
															OutlinePosition,
															UDim2.new(0, 86, 0, ((#Menu.Inputs.Modes * 16) + 6)),
															"Lighter Background",
															nil,
															nil,
															3
														)
													--
													Content.Objects["Glow"] = Menu.Objects:Create("ImageLabel", {
														BackgroundTransparency = 1,
														BorderSizePixel = 0,
														Parent = Content.Objects["Outline"],
														Position = UDim2.new(0.5, 0, 0.5, 0),
														Size = UDim2.new(1, 48, 1, 48),
														ImageColor3 = Color3.fromRGB(80, 80, 80),
														ScaleType = "Slice",
														ZIndex = 2,
														SliceCenter = Rect.new(49, 49, 450, 450),
														ImageTransparency = 0.6,
														AnchorPoint = Vector2.new(0.5, 0.5),
														ImageColor3 = "Accent",
														Image = "rbxassetid://6015897843",
													})
													--
													Content.Objects["List"] =
														Menu.Objects:List(nil, Content.Objects["Frame"], nil, nil, nil)
													Content.Objects["Padding"] = Menu.Objects:Padding(
														Content.Objects["List"],
														UDim.new(0, 2),
														UDim.new(0, 2)
													)
													--
													do -- Options
														for Index, Value in next, Menu.Inputs.Modes do
															local Item = {
																Objects = {},
																--
																State = (Self.Mode == Value),
															}
															--
															do -- Objects
																Item.Objects["Holder"] = Menu.Objects:Holder(
																	nil,
																	Content.Objects["Frame"],
																	nil,
																	UDim2.new(1, 0, 0, 16),
																	nil,
																	3
																)
																--
																Item.Objects["Text"] = Menu.Objects:Text(
																	false,
																	nil,
																	Item.Objects["Holder"],
																	UDim2.new(0, 0, 0, 0),
																	UDim2.new(1, 0, 1, 0),
																	Value,
																	nil,
																	"Center",
																	3
																)
																--
																Item.Objects["Button"] =
																	Menu.Objects:Button(Item.Objects["Holder"], 3)
															end
															--
															do -- Functions
																function Item:Set(State)
																	Item.State = State
																	--
																	Menu.Objects:Update(Item.Objects["Text"], {
																		TextColor3 = (
																			Item.State and "Accent" or "Light Text"
																		),
																	})
																end
															end
															--
															do -- Connections
																Content.Connections[#Content.Connections + 1] = Utils:Connect(
																	Item.Objects["Button"].MouseButton1Down,
																	function()
																		if not Item.State then
																			Self:Adjust(Value)
																			--
																			for Index2, Value2 in next, Content.Items do
																				Value2:Set(Value2 == Item)
																			end
																			--
																			Content:Close()
																		end
																	end
																)
															end
															--
															do -- Setup
																if Item.State then
																	Item:Set(true)
																end
																--
																Content.Items[Index] = Item
															end
														end
													end
												end
												--
												do -- Functions
													function Content:Close()
														for Index, Value in next, Content.Connections do
															Value:Disconnect()
														end
														--
														if Content.Objects["Outline"] then
															Content.Objects["Outline"]:Remove()
														end
														--
														Content.Connections = nil
														Content.Objects = nil
														--
														Content.Items = nil
														Content.Init = nil
														Content.Tick = nil
														--
														Content = nil
														--
														Window.Opened[Type][Self] = nil
													end
													--
													function Content:Input()
														if Content.Init or ((tick() - Content.Tick) >= 0.05) then
															local Location = (Utils:Location() - Window.Inset)
															--
															local ContentPosition = (
																Content.Objects["Frame"].AbsolutePosition
																+ Vector2.new(0, 2)
															)
															local ContentSize = (
																Content.Objects["Frame"].AbsoluteSize - Vector2.new(0, 4)
															)
															--
															if
																not Utils.Math:Over(Location, ContentPosition, ContentSize)
															then
																Content:Close()
															end
														end
														--
														if Content then
															Content.Init = true
														end
													end
												end
												--
												do -- Connections
													Content.Connections[#Content.Connections + 1] = Utils:Connect(
														Services.UserInputService.InputBegan,
														function(Input)
															if Input.UserInputType == Enum.UserInputType.MouseButton1 then
																Content:Input(true)
															end
														end
													)
													--
													Content.Connections[#Content.Connections + 1] = Utils:Connect(
														Services.UserInputService.InputChanged,
														function(Input)
															if Input.UserInputType == Enum.UserInputType.MouseWheel then
																Content:Close()
															end
														end
													)
												end
												--
												do -- Setup
													Window.Opened[Type][Self] = Content
												end
											end
											--
											function Window.Content.CopyAndPaste(Self, Type)
												local Content = {
													Connections = {},
													Objects = {},
													Items = {},
													--
													Tick = tick(),
												}
												--
												do -- Init
													Window:Scroll(Self.Section.Objects["Content"], Self.Objects["Holder"])
												end
												--
												do -- Objects
													local SelfPosition = (
														Self.Objects["Holder"].AbsolutePosition
														- Self.Section.Objects["Content"].AbsolutePosition
													)
													local OutlinePosition = UDim2.new(
														0,
														(SelfPosition.X + 1),
														0,
														math.floor(
															SelfPosition.Y + Self.Objects["Holder"].AbsoluteSize.Y + 2
														)
													)
													--
													Content.Objects["Outline"], Content.Objects["Frame"] =
														Menu.Objects:Outline(
															nil,
															Self.Section.Objects["Open"],
															OutlinePosition,
															UDim2.new(0, 86, 0, ((2 * 16) + 6)),
															"Lighter Background",
															nil,
															nil,
															3
														)
													--
													Content.Objects["Glow"] = Menu.Objects:Create("ImageLabel", {
														BackgroundTransparency = 1,
														BorderSizePixel = 0,
														Parent = Content.Objects["Outline"],
														Position = UDim2.new(0.5, 0, 0.5, 0),
														Size = UDim2.new(1, 48, 1, 48),
														ImageColor3 = Color3.fromRGB(80, 80, 80),
														ScaleType = "Slice",
														ZIndex = 2,
														SliceCenter = Rect.new(49, 49, 450, 450),
														ImageTransparency = 0.6,
														AnchorPoint = Vector2.new(0.5, 0.5),
														ImageColor3 = "Accent",
														Image = "rbxassetid://6015897843",
													})
													--
													Content.Objects["List"] =
														Menu.Objects:List(nil, Content.Objects["Frame"], nil, nil, nil)
													Content.Objects["Padding"] = Menu.Objects:Padding(
														Content.Objects["List"],
														UDim.new(0, 2),
														UDim.new(0, 2)
													)
													--
													do -- Options
														for Index, Value in next, { "Copy", "Paste" } do
															local Item = {
																Objects = {},
															}
															--
															do -- Objects
																Item.Objects["Holder"] = Menu.Objects:Holder(
																	nil,
																	Content.Objects["Frame"],
																	nil,
																	UDim2.new(1, 0, 0, 16),
																	nil,
																	3
																)
																--
																Item.Objects["Text"] = Menu.Objects:Text(
																	false,
																	nil,
																	Item.Objects["Holder"],
																	UDim2.new(0, 0, 0, 0),
																	UDim2.new(1, 0, 1, 0),
																	Value,
																	nil,
																	"Center",
																	3
																)
																--
																Item.Objects["Button"] =
																	Menu.Objects:Button(Item.Objects["Holder"], 3)
															end
															--
															do -- Connections
																Content.Connections[#Content.Connections + 1] = Utils:Connect(
																	Item.Objects["Button"].MouseButton1Down,
																	function()
																		if Value == "Copy" then
																			Self.Window.CopiedColor = Self:Get()
																			--
																			setclipboard(Self.Window.CopiedColor:ToHex())
																			Content:Close()
																		elseif Value == "Paste" then
																			if Self.Window.CopiedColor then
																				local _, Alpha = Self:Get()
																				--
																				Self:Set(Self.Window.CopiedColor, Alpha)
																			end
																			--
																			Content:Close()
																		end
																	end
																)
															end
															--
															do -- Setup
																Content.Items[Index] = Item
															end
														end
													end
												end
												--
												do -- Functions
													function Content:Close()
														for Index, Value in next, Content.Connections do
															Value:Disconnect()
														end
														--
														if Content.Objects["Outline"] then
															Content.Objects["Outline"]:Remove()
														end
														--
														Content.Connections = nil
														Content.Objects = nil
														--
														Content.Items = nil
														Content.Init = nil
														Content.Tick = nil
														--
														Content = nil
														--
														Window.Opened[Type][Self] = nil
													end
													--
													function Content:Input()
														if Content.Init or ((tick() - Content.Tick) >= 0.05) then
															local Location = (Utils:Location() - Window.Inset)
															--
															local ContentPosition = (
																Content.Objects["Frame"].AbsolutePosition
																+ Vector2.new(0, 2)
															)
															local ContentSize = (
																Content.Objects["Frame"].AbsoluteSize - Vector2.new(0, 4)
															)
															--
															if
																not Utils.Math:Over(Location, ContentPosition, ContentSize)
															then
																Content:Close()
															end
														end
														--
														if Content then
															Content.Init = true
														end
													end
												end
												--
												do -- Connections
													Content.Connections[#Content.Connections + 1] = Utils:Connect(
														Services.UserInputService.InputBegan,
														function(Input)
															if Input.UserInputType == Enum.UserInputType.MouseButton1 then
																Content:Input(true)
															end
														end
													)
													--
													Content.Connections[#Content.Connections + 1] = Utils:Connect(
														Services.UserInputService.InputChanged,
														function(Input)
															if Input.UserInputType == Enum.UserInputType.MouseWheel then
																Content:Close()
															end
														end
													)
												end
												--
												do -- Setup
													Window.Opened[Type][Self] = Content
												end
											end
										end
									end
									--
									do -- Connections
										if not Window.Self then
											Utils:Connect(Window.Objects["Button"].MouseButton1Down, function()
												local Location = Utils:Location()
												local Position = Window.Objects["Screen_Outline"].AbsolutePosition
												--
												Window.Drag = (Location - Position)
												--
												Window:Reposition(Location)
											end)
										end
										--
										Utils:Connect(Services.UserInputService.InputBegan, function(Input)
											Utils:Thread(
												Window.Update,
												(Input.KeyCode.Name ~= "Unknown" and Input.KeyCode or Input.UserInputType),
												true
											)
										end)
										--
										Utils:Connect(Services.UserInputService.InputEnded, function(Input)
											if Input.UserInputType == Enum.UserInputType.MouseButton1 then
												for Index, Value in next, Window.Holds do
													Index[Value] = nil
													--
													Window.Holds[Index] = nil
												end
											end
											--
											if Window.Drag and Input.UserInputType == Enum.UserInputType.MouseButton1 then
												Window.Drag = nil
											end
											--
											Utils:Thread(
												Window.Update,
												(Input.KeyCode.Name ~= "Unknown" and Input.KeyCode or Input.UserInputType)
											)
										end)
										--
										Utils:Connect(Services.UserInputService.InputChanged, function(Input)
											if Window.Drag then
												Window:Reposition(Utils:Location())
											end
											--
											for Index, Value in next, Window.Holds do
												Index:Refresh()
											end
										end)
									end
									--
									do -- Setup
										return setmetatable(Window, {
											__index = Menu,
										})
									end
								end
								--
								function Menu:Watermark(Properties)
									local Properties = (Properties or {})
									local Watermark = {
										Objects = {},
										--
										State = true,
										Position = 2,
										Top = true,
										--
										Tick = tick(),
										--
										Window = self,
										--
										Refresh = (
											Utils:Property(Properties, "RefreshRate", "Refresh", "Rate", "Tick") or 10
										),
										Callback = (Utils:Property(Properties, "Callback", "Function", "Func", "Update")),
									}
									--
									do -- Objects
										Watermark.Objects["Outline"], Watermark.Objects["Frame"] = Menu.Objects:Outline(
											Vector2.new(1, 0),
											Watermark.Window.Objects["Screen"],
											UDim2.new(1, -(54 + 8), 0, 8),
											UDim2.new(0, 200, 0, 25),
											"Light Background",
											"Dark Inline",
											nil,
											1000
										)
										--

										--
										Watermark.Objects["Screen_Glow"] = Menu.Objects:Create("ImageLabel", {
											BackgroundTransparency = 1,
											BorderSizePixel = 0,
											Parent = Watermark.Objects["Outline"],
											Position = UDim2.new(0.5, 0, 0.5, 0),
											Size = UDim2.new(1, 24, 1, 24),
											ImageColor3 = Color3.fromRGB(80, 80, 80),
											ScaleType = "Slice",
											ZIndex = -1,
											SliceCenter = Rect.new(49, 49, 450, 450),
											ImageTransparency = 0.6,
											AnchorPoint = Vector2.new(0.5, 0.5),
											ImageColor3 = "Accent",
											Image = "rbxassetid://6015897843",
										})
										--
										Watermark.Objects["Gradient"] = Menu.Objects:Holder(
											nil,
											Watermark.Objects["Frame"],
											UDim2.new(0, 1, 0, 1),
											UDim2.new(1, -2, 0, 2)
										)
										--
										Watermark.Objects["Gradient_Line1"] = Menu.Objects:Frame(
											nil,
											"Accent",
											nil,
											nil,
											nil,
											0,
											Watermark.Objects["Gradient"],
											nil,
											UDim2.new(1, 0, 0, 2),
											nil,
											1000
										)
										Watermark.Objects["Gradient_Line2"] = Menu.Objects:Frame(
											nil,
											"Outline",
											0.45,
											nil,
											nil,
											0,
											Watermark.Objects["Gradient"],
											UDim2.new(0, 0, 0, 1),
											UDim2.new(1, 0, 0, 1),
											nil,
											1000
										)
										--
										Watermark.Objects["Text_Outline"] = Menu.Objects:Text(
											false,
											Vector2.new(0, 0.5),
											Watermark.Objects["Frame"],
											UDim2.new(0, 1, 0.5, 2),
											UDim2.new(1, 0, 1, -6),
											"",
											"Outline",
											"Center",
											1000
										)
										Watermark.Objects["Text"] = Menu.Objects:Text(
											false,
											nil,
											Watermark.Objects["Text_Outline"],
											UDim2.new(0, -1, 0, -1),
											UDim2.new(1, 0, 1, 0),
											"",
											"Light Text",
											"Center",
											1000
										)

										local UserInputService = game:GetService("UserInputService")
										local RunService = game:GetService("RunService")

										local frame = Watermark.Objects["Frame"] and Watermark.Objects["Outline"]
										local dragging = false
										local dragStart, startPos
										local targetPos = frame.Position

										local function lerp(a, b, t)
											return a + (b - a) * t
										end

										RunService.RenderStepped:Connect(function()
											frame.Position = UDim2.new(
												lerp(frame.Position.X.Scale, targetPos.X.Scale, 0.2),
												lerp(frame.Position.X.Offset, targetPos.X.Offset, 0.2),
												lerp(frame.Position.Y.Scale, targetPos.Y.Scale, 0.2),
												lerp(frame.Position.Y.Offset, targetPos.Y.Offset, 0.2)
											)
										end)

										frame.InputBegan:Connect(function(input)
											if input.UserInputType == Enum.UserInputType.MouseButton1 then
												dragging = true
												dragStart = input.Position
												startPos = frame.Position
											end
										end)

										frame.InputEnded:Connect(function(input)
											if input.UserInputType == Enum.UserInputType.MouseButton1 then
												dragging = false
											end
										end)

										UserInputService.InputChanged:Connect(function(input)
											if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
												local delta = input.Position - dragStart
												targetPos = UDim2.new(
													startPos.X.Scale,
													startPos.X.Offset + delta.X,
													startPos.Y.Scale,
													startPos.Y.Offset + delta.Y
												)
											end
										end)
									end
									--
									do -- Functions
										function Watermark:Update()
											local Passed, Statement = Utils:Call(Watermark.Callback)
											--
											if Passed and Statement then
												Watermark.Objects["Outline"].Visible = true
												--
												Watermark.Objects["Text_Outline"].Text = Statement
												Watermark.Objects["Text"].Text = Statement
												--
												Watermark.Objects["Outline"].Size = UDim2.new(
													0,
													(Watermark.Objects["Text_Outline"].TextBounds.X + 14),
													0,
													25
												)
											else
												Watermark.Objects["Outline"].Visible = false
											end
										end
										--
										function Watermark:Set(State)
											Watermark.State = (typeof(State) == nil and Watermark.State or State)
											--
											Watermark.Objects["Outline"].Visible = Watermark.State
										end
										--

										--
										function Watermark:Gradient(State)
											Watermark.Top = State
											--
											Watermark.Objects["Text_Outline"].Position =
												UDim2.new(0, 1, 0.5, (Watermark.Top and 2 or 0))
											Watermark.Objects["Gradient"].Position =
												UDim2.new(0, 1, 0, (Watermark.Top and 1 or 18))
											Watermark.Objects["Gradient_Line2"].Position =
												UDim2.new(0, 0, 0, (Watermark.Top and 1 or 0))
										end
									end
									--
									do -- Connections
										Utils:Connect(
											Services.RunService.RenderStepped,
											LPH_NO_VIRTUALIZE(function()
												if Watermark.State then
													local Tick = tick()
													--
													if ((Tick - Watermark.Tick) * 1000) >= Watermark.Refresh then
														Watermark:Update()
														--
														Watermark.Tick = Tick
													end
												end
											end)
										)
									end
									--
									do -- Setup
										Flags["Watermark"] = Watermark
										--
										Watermark:Update()
										--
										return Watermark
									end
								end
								--
								function Menu:Toast(Properties)
									local Properties = Properties or {}
									local Toast = {
										Objects = {},
										State = true,
										Window = self,
										Text = (
											Utils:Property(Properties, "Text", "Message", "Message") or "TOAST MESSAGE"
										),
										Duration = (Utils:Property(Properties, "Duration", "Time", "Tick") or 5),
										Callback = (Utils:Property(Properties, "Callback", "Function", "Func", "Update")),
									}
									--
									do -- Objects
										Toast.Objects["Outline"], Toast.Objects["Frame"] = Menu.Objects:Outline(
											Vector2.new(0, 0),
											Toast.Window.Objects["Screen"] or Screen,
											UDim2.new(0, 50, 0, 50),
											UDim2.new(0, 250, 0, 30),
											"Light Background",
											"Dark Inline",
											nil,
											1000
										)
										--
										Toast.Objects["Glow"] = Menu.Objects:Create("ImageLabel", {
											BackgroundTransparency = 1,
											BorderSizePixel = 0,
											Parent = Toast.Objects["Frame"],
											Position = UDim2.new(0.5, 0, 0.5, 0),
											Size = UDim2.new(1, 38, 1, 38),
											ImageColor3 = Color3.fromRGB(80, 80, 80),
											ScaleType = "Slice",
											ZIndex = -1,
											SliceCenter = Rect.new(49, 49, 450, 450),
											ImageTransparency = 0.6,
											AnchorPoint = Vector2.new(0.5, 0.5),
											ImageColor3 = "Accent",
											Image = "rbxassetid://6015897843",
										})
										--
										Toast.Objects["Gradient"] = Menu.Objects:Holder(
											nil,
											Toast.Objects["Frame"],
											UDim2.new(0, 1, 0, 1),
											UDim2.new(1, -2, 0, 2)
										)
										--
										Toast.Objects["Gradient_Line1"] = Menu.Objects:Frame(
											nil,
											"Accent",
											nil,
											nil,
											nil,
											0,
											Toast.Objects["Gradient"],
											nil,
											UDim2.new(1, 0, 0, 2),
											nil,
											1000
										)
										--
										Toast.Objects["Gradient_Line2"] = Menu.Objects:Frame(
											nil,
											"Outline",
											0.45,
											nil,
											nil,
											0,
											Toast.Objects["Gradient"],
											UDim2.new(0, 0, 0, 1),
											UDim2.new(1, 0, 0, 1),
											nil,
											1000
										)
										--
										Toast.Objects["Text_Outline"] = Menu.Objects:Text(
											false,
											Vector2.new(0, 0.5),
											Toast.Objects["Frame"],
											UDim2.new(0, 1, 0.5, 2),
											UDim2.new(1, 0, 1, -6),
											"",
											"Outline",
											"Center",
											1000
										)
										--
										Toast.Objects["Text"] = Menu.Objects:Text(
											false,
											nil,
											Toast.Objects["Text_Outline"],
											UDim2.new(0, -1, 0, -1),
											UDim2.new(1, 0, 1, 0),
											Toast.Text,
											"Light Text",
											"Center",
											1000
										)
									end
									--
									do -- Position
										local ScreenHeight = Services.Workspace.CurrentCamera.ViewportSize.Y
										local HalfHeight = ScreenHeight * 0.5
										--
										local ActiveCount = #self.ActiveToasts
										local Offset = 100 + (ActiveCount * 35)
										--
										if Offset > HalfHeight then
											table.insert(self.PendingToasts, Toast)
											Toast.State = false
										else
											table.insert(self.ActiveToasts, Toast)
											--
											Toast.Objects["Outline"].AnchorPoint = Vector2.new(0.5, 1)
											Toast.Objects["Outline"].Position = UDim2.new(0.5, 0, 1, -Offset)
										end
									end
									--
									do -- Functions
										function Toast:StartTimer()
											local Tween = Services.TweenService:Create(
												Toast.Objects["Gradient_Line1"],
												TweenInfo.new(
													Toast.Duration,
													Enum.EasingStyle.Linear,
													Enum.EasingDirection.In
												),
												{ Size = UDim2.new(0, 0, 0, 2) }
											)
											Tween:Play()
											Tween.Completed:Connect(function()
												Toast:Remove()
											end)
										end
										--
										function Toast:Remove()
											if not Toast.State then
												return
											end
											--
											Toast.State = false
											--
											if Toast.Callback then
												Utils:Call(Toast.Callback)
											end
											--
											for _, Value in pairs(Toast.Objects) do
												if Value and Value.Destroy then
													Value:Destroy()
												end
											end
											--
											for Index, Value in ipairs(Toast.Window.ActiveToasts) do
												if Value == Toast then
													table.remove(Toast.Window.ActiveToasts, Index)
													break
												end
											end
											--
											local ScreenHeight = Services.Workspace.CurrentCamera.ViewportSize.Y
											local HalfHeight = ScreenHeight * 0.5
											--
											for Index, Value in ipairs(Toast.Window.ActiveToasts) do
												local NewOffset = 100 + ((Index - 1) * 35)
												if NewOffset > HalfHeight then
													NewOffset = HalfHeight
												end
												local Tween = Services.TweenService:Create(
													Value.Objects["Outline"],
													TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
													{ Position = UDim2.new(0.5, 0, 1, -NewOffset) }
												)
												Tween:Play()
											end
											--
											while #Toast.Window.PendingToasts > 0 do
												local ActiveCount = #Toast.Window.ActiveToasts
												local NextOffset = 100 + (ActiveCount * 35)

												if NextOffset > HalfHeight then
													break
												end
												--
												local NextToast = table.remove(Toast.Window.PendingToasts, 1)
												if NextToast then
													NextToast.State = true
													table.insert(Toast.Window.ActiveToasts, NextToast)
													--
													NextToast.Objects["Outline"].AnchorPoint = Vector2.new(0.5, 1)
													NextToast.Objects["Outline"].Position =
														UDim2.new(0.5, 0, 1, -NextOffset)
													--
													NextToast:StartTimer()
												end
											end
										end
									end
									--
									do -- Setup
										if Toast.State then
											task.spawn(function()
												task.wait(5)
												--
												if Toast.Objects and Toast.Objects["Outline"] and Toast.Objects["Text"] then
													Toast.Objects["Outline"].Size = UDim2.new(
														0,
														math.max(Toast.Objects["Text"].TextBounds.X + 14, 80),
														0,
														30
													)
												end
											end)
											--
											Toast:StartTimer()
										end
									end
									--
									return Toast
								end
								--
								function Menu:Indicators()
									local Binds = {
										Objects = {},
										Items = {},
										--
										Refresh = 5,
										State = true,
										--
										Tick = tick(),
										--
										Window = self,
									}
									--
									do -- Objects
										Binds.Objects["Outline"], Binds.Objects["Frame"] = Menu.Objects:Outline(
											Vector2.new(0, 0.5),
											Binds.Window.Objects["Screen"],
											UDim2.new(0, 8, 0.5, 0),
											UDim2.new(0, 175, 0, 200),
											"Light Background",
											"Dark Inline",
											nil,
											1000
										)

										local UserInputService = game:GetService("UserInputService")
										local RunService = game:GetService("RunService")

										local frame = Binds.Objects["Frame"] and Binds.Objects["Outline"]
										local dragging = false
										local dragStart, startPos
										local targetPos = frame.Position

										local function lerp(a, b, t)
											return a + (b - a) * t
										end

										RunService.RenderStepped:Connect(function()
											frame.Position = UDim2.new(
												lerp(frame.Position.X.Scale, targetPos.X.Scale, 0.2),
												lerp(frame.Position.X.Offset, targetPos.X.Offset, 0.2),
												lerp(frame.Position.Y.Scale, targetPos.Y.Scale, 0.2),
												lerp(frame.Position.Y.Offset, targetPos.Y.Offset, 0.2)
											)
										end)

										frame.InputBegan:Connect(function(input)
											if input.UserInputType == Enum.UserInputType.MouseButton1 then
												dragging = true
												dragStart = input.Position
												startPos = frame.Position
											end
										end)

										frame.InputEnded:Connect(function(input)
											if input.UserInputType == Enum.UserInputType.MouseButton1 then
												dragging = false
											end
										end)

										UserInputService.InputChanged:Connect(function(input)
											if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
												local delta = input.Position - dragStart
												targetPos = UDim2.new(
													startPos.X.Scale,
													startPos.X.Offset + delta.X,
													startPos.Y.Scale,
													startPos.Y.Offset + delta.Y
												)
											end
										end)
										--
										Binds.Objects["Screen_Glow"] = Menu.Objects:Create("ImageLabel", {
											BackgroundTransparency = 1,
											BorderSizePixel = 0,
											Parent = Binds.Objects["Outline"],
											Position = UDim2.new(0.5, 0, 0.5, 0),
											Size = UDim2.new(1, 48, 1, 48),
											ImageColor3 = Color3.fromRGB(80, 80, 80),
											ScaleType = "Slice",
											ZIndex = -1,
											SliceCenter = Rect.new(49, 49, 450, 450),
											ImageTransparency = 0.6,
											AnchorPoint = Vector2.new(0.5, 0.5),
											ImageColor3 = "Accent",
											Image = "rbxassetid://6015897843",
										})
										--
										Binds.Objects["Gradient"] = Menu.Objects:Holder(
											nil,
											Binds.Objects["Frame"],
											UDim2.new(0, 1, 0, 1),
											UDim2.new(1, -2, 0, 2)
										)
										--
										Binds.Objects["Gradient_Line1"] = Menu.Objects:Frame(
											nil,
											"Accent",
											nil,
											nil,
											nil,
											0,
											Binds.Objects["Gradient"],
											nil,
											UDim2.new(1, 0, 0, 2),
											nil,
											1000
										)
										Binds.Objects["Gradient_Line2"] = Menu.Objects:Frame(
											nil,
											"Outline",
											0.45,
											nil,
											nil,
											0,
											Binds.Objects["Gradient"],
											UDim2.new(0, 0, 0, 1),
											UDim2.new(1, 0, 0, 1),
											nil,
											1000
										)
										--
										Binds.Objects["Text_Outline"] = Menu.Objects:Text(
											true,
											Vector2.new(0, 0),
											Binds.Objects["Frame"],
											UDim2.new(0, 0, 0, 5),
											UDim2.new(1, 0, 0, 15),
											"",
											"Outline",
											"Center",
											1000
										)
										Binds.Objects["Text"] = Menu.Objects:Text(
											true,
											nil,
											Binds.Objects["Text_Outline"],
											UDim2.new(0, -1, 0, -1),
											UDim2.new(1, 0, 1, 0),
											"Binds",
											"Light Text",
											"Center",
											1000
										)
										--
										Binds.Objects["Holder_Outline"], Binds.Objects["Holder_Frame"] =
											Menu.Objects:Outline(
												nil,
												Binds.Objects["Frame"],
												UDim2.new(0, 2, 0, 22),
												UDim2.new(1, -4, 1, -24),
												"Lighter Background",
												nil,
												nil,
												1000
											)
										--
										Binds.Objects["Holder"] = Menu.Objects:Holder(
											nil,
											Binds.Objects["Holder_Frame"],
											UDim2.new(0, 4, 0, 2),
											UDim2.new(1, -8, 1, -4)
										)
										--
										Menu.Objects:List(nil, Binds.Objects["Holder"], nil, nil, nil)
									end
									--
									do -- Functions
										function Binds:Insert(Content)
											local Item = {
												Objects = {},
												--
												Active = false,
											}
											--
											do -- Objects
												Item.Objects["Holder"] = Menu.Objects:Holder(
													nil,
													Binds.Objects["Holder"],
													UDim2.new(0, 0, 0, 0),
													UDim2.new(1, 0, 0, 17),
													false
												)
												--
												Item.Objects["Name"] = Menu.Objects:Text(
													false,
													nil,
													Item.Objects["Holder"],
													UDim2.new(0, 0, 0, 0),
													UDim2.new(1, 0, 1, 0),
													Content.Name,
													"Light Text",
													nil,
													1000
												)
												Item.Objects["State"] = Menu.Objects:Text(
													false,
													nil,
													Item.Objects["Holder"],
													UDim2.new(0, 0, 0, 0),
													UDim2.new(1, 0, 1, 0),
													"",
													"Light Text",
													"Right",
													1000
												)
											end
											--
											do -- Functions
												function Item:Update()
													self.Active = (typeof(Content.Short) == "string")
													--
													if Content.Short then
														self.Objects["Holder"].Visible = true
														--
														self.Objects["State"].Text = string.format("%s", Content.Short)
														--
														Menu.Objects:Update(
															self.Objects["State"],
															{ TextColor3 = Content.Active and "Accent" or "Light Text" }
														)
													else
														self.Objects["Holder"].Visible = false
													end
													--
													Binds:Update()
												end
											end
											--
											do -- Setup
												Binds.Items[#Binds.Items + 1] = Item
												--
												return Item
											end
										end
										--
										function Binds:Update()
											local Count = 0
											--
											for Index, Item in next, self.Items do
												if Item.Active then
													Count += 1
												end
											end
											--
											Binds.Objects["Outline"].Size = UDim2.new(0, 175, 0, ((Count * 18) + 28 + 5))
										end
										--
										function Binds:Set(State)
											Binds.State = (typeof(State) == nil and Binds.State or State)
											--
											Binds.Objects["Outline"].Visible = Binds.State
										end
									end
									--
									do -- Setup
										Flags["Binds"] = Binds
										--
										return Binds
									end
								end
								--
								do -- viewers
									function Menu:Belt()
										Belt = {
											Objects = {},
											Items = {},
											Refresh = 5,
											State = true,
											Tick = tick(),
											Window = self,
										}

										local Players = game:GetService("Players")
										local RunService = game:GetService("RunService")
										local UserInputService = game:GetService("UserInputService")
										local LocalPlayer = Players.LocalPlayer
										local Camera = workspace.CurrentCamera
										local ReplicatedStorage = game:GetService("ReplicatedStorage")

										Belt.Objects["Outline"], Belt.Objects["Frame"] = Menu.Objects:Outline(
											Vector2.new(0, 0.5),
											Belt.Window.Objects["Screen"],
											UDim2.new(0.57, 0, 0.05, 0),
											UDim2.new(0, 200, 0, 100),
											"Light Background",
											"Dark Inline",
											nil,
											1000
										)
										Belt.Objects["Outline"].AnchorPoint = Vector2.new(0.5, 0)

										local frame = Belt.Objects["Outline"]
										local dragging = false
										local dragStart, startPos
										local targetPos = frame.Position

										local function lerp(a, b, t)
											return a + (b - a) * t
										end

										RunService.RenderStepped:Connect(function()
											frame.Position = UDim2.new(
												lerp(frame.Position.X.Scale, targetPos.X.Scale, 0.2),
												lerp(frame.Position.X.Offset, targetPos.X.Offset, 0.2),
												lerp(frame.Position.Y.Scale, targetPos.Y.Scale, 0.2),
												lerp(frame.Position.Y.Offset, targetPos.Y.Offset, 0.2)
											)
										end)

										frame.InputBegan:Connect(function(input)
											if input.UserInputType == Enum.UserInputType.MouseButton1 then
												dragging = true
												dragStart = input.Position
												startPos = frame.Position
											end
										end)

										frame.InputEnded:Connect(function(input)
											if input.UserInputType == Enum.UserInputType.MouseButton1 then
												dragging = false
											end
										end)

										UserInputService.InputChanged:Connect(function(input)
											if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
												local delta = input.Position - dragStart
												targetPos = UDim2.new(
													startPos.X.Scale,
													startPos.X.Offset + delta.X,
													startPos.Y.Scale,
													startPos.Y.Offset + delta.Y
												)
											end
										end)

										Belt.Objects["Screen_Glow"] = Menu.Objects:Create("ImageLabel", {
											BackgroundTransparency = 1,
											BorderSizePixel = 0,
											Parent = Belt.Objects["Outline"],
											Position = UDim2.new(0.5, 0, 0.5, 0),
											Size = UDim2.new(1, 48, 1, 48),
											ImageColor3 = Color3.fromRGB(80, 80, 80),
											ScaleType = "Slice",
											ZIndex = -1,
											SliceCenter = Rect.new(49, 49, 450, 450),
											ImageTransparency = 0.6,
											AnchorPoint = Vector2.new(0.5, 0.5),
											ImageColor3 = "Accent",
											Image = "rbxassetid://6015897843",
										})

										Belt.Objects["Gradient"] = Menu.Objects:Holder(
											nil,
											Belt.Objects["Frame"],
											UDim2.new(0, 1, 0, 1),
											UDim2.new(1, -2, 0, 2)
										)

										Belt.Objects["Gradient_Line1"] = Menu.Objects:Frame(
											nil,
											"Accent",
											nil,
											nil,
											nil,
											0,
											Belt.Objects["Gradient"],
											nil,
											UDim2.new(1, 0, 0, 2),
											nil,
											1000
										)
										Belt.Objects["Gradient_Line2"] = Menu.Objects:Frame(
											nil,
											"Outline",
											0.45,
											nil,
											nil,
											0,
											Belt.Objects["Gradient"],
											UDim2.new(0, 0, 0, 1),
											UDim2.new(1, 0, 0, 1),
											nil,
											1000
										)

										Belt.Objects["Text_Outline"] = Menu.Objects:Text(
											true,
											Vector2.new(0, 0),
											Belt.Objects["Frame"],
											UDim2.new(0, 0, 0, 5),
											UDim2.new(1, 0, 0, 15),
											"",
											"Outline",
											"Center",
											1000
										)
										Belt.Objects["Text"] = Menu.Objects:Text(
											true,
											nil,
											Belt.Objects["Text_Outline"],
											UDim2.new(0, -1, 0, -1),
											UDim2.new(1, 0, 1, 0),
											"Belt",
											"Light Text",
											"Center",
											1000
										)

										Belt.Objects["Holder_Outline"], Belt.Objects["Holder_Frame"] = Menu.Objects:Outline(
											nil,
											Belt.Objects["Frame"],
											UDim2.new(0, 2, 0, 22),
											UDim2.new(1, -4, 1, -24),
											"Lighter Background",
											nil,
											nil,
											1000
										)

										Belt.Objects["Holder"] = Menu.Objects:Holder(
											nil,
											Belt.Objects["Holder_Frame"],
											UDim2.new(0, 4, 0, 2),
											UDim2.new(1, -8, 1, -4)
										)

										local container = Instance.new("Frame")
										container.Name = "ImageContainer"
										container.BackgroundTransparency = 1
										container.Size = UDim2.new(1, 0, 0, 64)
										container.Position = UDim2.new(0, 0, 0, 8)
										container.Parent = Belt.Objects["Holder"]

										local layout = Instance.new("UIListLayout")
										layout.Parent = container
										layout.FillDirection = Enum.FillDirection.Horizontal
										layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
										layout.VerticalAlignment = Enum.VerticalAlignment.Center
										layout.Padding = UDim.new(0, 10)

										local slotImages = {}
										for i = 1, 3 do
											local img = Instance.new("ImageLabel")
											img.Name = "Slot" .. i
											img.Parent = container
											img.BackgroundTransparency = 1
											img.Size = UDim2.new(0, 50, 0, 50)
											img.Image = "rbxassetid://0"
											img.ScaleType = Enum.ScaleType.Fit
											img.ZIndex = 1001
											slotImages[i] = img
										end

										local RS = ReplicatedStorage
										local ItemsListModels = RS:WaitForChild("ItemsList", 5)

										local allowedItems = {
											AsVal = true,
											MP443 = true,
											PPSH41 = true,
											SVD = true,
											PKM = true,
											Mosin = true,
											Makarov = true,
											AKM = true,
											TT33 = true,
											IZh12 = true,
											MP5SD = true,
											VZ61 = true,
											Saiga12 = true,
											TFZ98S = true,
											IZh81 = true,
											AKMN = true,
											R700 = true,
											FAL = true,
											M4 = true,
											ADAR15 = true,
											SKS = true,
											RPG7 = true,
										}

										local function GetItemIconFromModels(itemName)
											if not ItemsListModels or not itemName then
												return nil
											end

											local model = ItemsListModels:FindFirstChild(itemName)
											if not model then
												for _, child in ipairs(ItemsListModels:GetChildren()) do
													if string.lower(child.Name):find(string.lower(itemName)) then
														model = child
														break
													end
												end
											end

											if model then
												for _, desc in ipairs(model:GetDescendants()) do
													if desc:IsA("ImageLabel") or desc:IsA("ImageButton") then
														if desc.Image and desc.Image ~= "" then
															return desc.Image
														end
													elseif desc:IsA("Decal") or desc:IsA("Texture") then
														if desc.Texture and desc.Texture ~= "" then
															return desc.Texture
														end
													elseif
														desc:IsA("StringValue")
														and string.find(string.lower(desc.Name), "icon")
													then
														return desc.Value
													end
												end
											end

											return nil
										end

										local function GetPlayerEquipmentImages(player)
											local icons = {}
											if not player then
												return icons
											end
											local RSPlayers = RS:FindFirstChild("Players")
											if not RSPlayers then
												return icons
											end
											local pdata = RSPlayers:FindFirstChild(player.Name)
											if not pdata then
												return icons
											end
											local inventory = pdata:FindFirstChild("Inventory")
											if not inventory then
												return icons
											end

											for _, item in ipairs(inventory:GetChildren()) do
												local itemName = item.Name
												if allowedItems[itemName] then
													local icon = GetItemIconFromModels(itemName)
													if icon then
														table.insert(icons, icon)
													end
													if #icons >= 3 then
														break
													end
												end
											end
											return icons
										end

										function closestplr()
											local closestPlayer, closestDistance = nil, math.huge
											local mousePos = UserInputService:GetMouseLocation()

											for _, player in pairs(Players:GetPlayers()) do
												if player ~= LocalPlayer and player.Character then
													local hrp = player.Character:FindFirstChild("HumanoidRootPart")
													if hrp then
														local screenPos, onScreen =
															Camera:WorldToViewportPoint(hrp.Position)
														if onScreen then
															local dist = (Vector2.new(screenPos.X, screenPos.Y) - mousePos).Magnitude
															if dist < closestDistance then
																closestDistance = dist
																closestPlayer = player
															end
														end
													end
												end
											end
											return closestPlayer
										end

										RunService.Heartbeat:Connect(function()
											local plr = closestplr()
											local imgs = plr and GetPlayerEquipmentImages(plr) or {}

											for i = 1, 3 do
												slotImages[i].Image = imgs[i] or "rbxassetid://0"
											end
										end)

										Menu.Objects:List(nil, Belt.Objects["Holder"], nil, nil, nil)
									end
									--
									function Menu:Armor()
										Armor = {
											Objects = {},
											Items = {},
											Refresh = 5,
											State = true,
											Tick = tick(),
											Window = self,
										}
										local Players = game:GetService("Players")
										local RunService = game:GetService("RunService")
										local UserInputService = game:GetService("UserInputService")
										local LocalPlayer = Players.LocalPlayer
										local Camera = workspace.CurrentCamera
										local ReplicatedStorage = game:GetService("ReplicatedStorage")
										Armor.Objects["Outline"], Armor.Objects["Frame"] = Menu.Objects:Outline(
											Vector2.new(0, 0.5),
											Armor.Window.Objects["Screen"],
											UDim2.new(0.8, 0, 0.05, 0),
											UDim2.new(0, 450, 0, 100),
											"Light Background",
											"Dark Inline",
											nil,
											1000
										)
										Armor.Objects["Outline"].AnchorPoint = Vector2.new(0.5, 0)
										local frame = Armor.Objects["Outline"]
										local dragging = false
										local dragStart, startPos
										local targetPos = frame.Position
										local function lerp(a, b, t)
											return a + (b - a) * t
										end
										RunService.RenderStepped:Connect(function()
											frame.Position = UDim2.new(
												lerp(frame.Position.X.Scale, targetPos.X.Scale, 0.2),
												lerp(frame.Position.X.Offset, targetPos.X.Offset, 0.2),
												lerp(frame.Position.Y.Scale, targetPos.Y.Scale, 0.2),
												lerp(frame.Position.Y.Offset, targetPos.Y.Offset, 0.2)
											)
										end)
										frame.InputBegan:Connect(function(input)
											if input.UserInputType == Enum.UserInputType.MouseButton1 then
												dragging = true
												dragStart = input.Position
												startPos = frame.Position
											end
										end)
										frame.InputEnded:Connect(function(input)
											if input.UserInputType == Enum.UserInputType.MouseButton1 then
												dragging = false
											end
										end)
										UserInputService.InputChanged:Connect(function(input)
											if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
												local delta = input.Position - dragStart
												targetPos = UDim2.new(
													startPos.X.Scale,
													startPos.X.Offset + delta.X,
													startPos.Y.Scale,
													startPos.Y.Offset + delta.Y
												)
											end
										end)
										Armor.Objects["Screen_Glow"] = Menu.Objects:Create("ImageLabel", {
											BackgroundTransparency = 1,
											BorderSizePixel = 0,
											Parent = Armor.Objects["Outline"],
											Position = UDim2.new(0.5, 0, 0.5, 0),
											Size = UDim2.new(1, 48, 1, 48),
											ImageColor3 = Color3.fromRGB(80, 80, 80),
											ScaleType = "Slice",
											ZIndex = -1,
											SliceCenter = Rect.new(49, 49, 450, 450),
											ImageTransparency = 0.6,
											AnchorPoint = Vector2.new(0.5, 0.5),
											ImageColor3 = "Accent",
											Image = "rbxassetid://6015897843",
										})
										Armor.Objects["Gradient"] = Menu.Objects:Holder(
											nil,
											Armor.Objects["Frame"],
											UDim2.new(0, 1, 0, 1),
											UDim2.new(1, -2, 0, 2)
										)
										Armor.Objects["Gradient_Line1"] = Menu.Objects:Frame(
											nil,
											"Accent",
											nil,
											nil,
											nil,
											0,
											Armor.Objects["Gradient"],
											nil,
											UDim2.new(1, 0, 0, 2),
											nil,
											1000
										)
										Armor.Objects["Gradient_Line2"] = Menu.Objects:Frame(
											nil,
											"Outline",
											0.45,
											nil,
											nil,
											0,
											Armor.Objects["Gradient"],
											UDim2.new(0, 0, 0, 1),
											UDim2.new(1, 0, 0, 1),
											nil,
											1000
										)
										Armor.Objects["Text_Outline"] = Menu.Objects:Text(
											true,
											Vector2.new(0, 0),
											Armor.Objects["Frame"],
											UDim2.new(0, 0, 0, 5),
											UDim2.new(1, 0, 0, 15),
											"",
											"Outline",
											"Center",
											1000
										)
										Armor.Objects["Text"] = Menu.Objects:Text(
											true,
											nil,
											Armor.Objects["Text_Outline"],
											UDim2.new(0, -1, 0, -1),
											UDim2.new(1, 0, 1, 0),
											"Armor",
											"Light Text",
											"Center",
											1000
										)
										Armor.Objects["Holder_Outline"], Armor.Objects["Holder_Frame"] =
											Menu.Objects:Outline(
												nil,
												Armor.Objects["Frame"],
												UDim2.new(0, 2, 0, 22),
												UDim2.new(1, -4, 1, -24),
												"Lighter Background",
												nil,
												nil,
												1000
											)
										Armor.Objects["Holder"] = Menu.Objects:Holder(
											nil,
											Armor.Objects["Holder_Frame"],
											UDim2.new(0, 4, 0, 2),
											UDim2.new(1, -8, 1, -4)
										)
										local container = Instance.new("Frame")
										container.Name = "ImageContainer"
										container.BackgroundTransparency = 1
										container.Size = UDim2.new(1, 0, 0, 64)
										container.Position = UDim2.new(0, 0, 0, 8)
										container.Parent = Armor.Objects["Holder"]
										local layout = Instance.new("UIListLayout")
										layout.Parent = container
										layout.FillDirection = Enum.FillDirection.Horizontal
										layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
										layout.VerticalAlignment = Enum.VerticalAlignment.Center
										layout.Padding = UDim.new(0, 10)
										local slotImages = {}
										for i = 1, 7 do
											local img = Instance.new("ImageLabel")
											img.Name = "Slot" .. i
											img.Parent = container
											img.BackgroundTransparency = 1
											img.Size = UDim2.new(0, 50, 0, 50)
											img.Image = "rbxassetid://0"
											img.ScaleType = Enum.ScaleType.Fit
											img.ZIndex = 1001
											slotImages[i] = img
										end
										local RS = ReplicatedStorage
										local ItemsListModels = RS:WaitForChild("ItemsList", 5)
										local allowedItems2 = {
											MotorcycleHelmet = true,
											SSH68 = true,
											["6B27"] = true,
											["6B47"] = true,
											Zsh = true,
											TORS = true,
											Altyn = true,
											FastMT = true,
											Bandoiler = true,
											["6B2"] = true,
											["6B23"] = true,
											Kulon = true,
											["6B5"] = true,
											["6B45"] = true,
											Smersh = true,
											IOTV = true,
											Lynx = true,
											WastelandBackpack = true,
											SpecopsBackpack = true,
											Tortilla = true,
											Attak5 = true,
											HandWraps = true,
											CombatGloves = true,
											Balaclava = true,
											GP5 = true,
											GhillieHood = true,
											LegArmor = true,
											HSPV = true,
											WastelandShirt = true,
											WastelandPants = true,
											CamoShirt = true,
											CamoPants = true,
											CivilianPants = true,
											CivilianShirt = true,
											GhillieShirt = true,
											GorkaShirt = true,
											GhilliePants = true,
											GorkaPants = true,
											JPC = true,
										}
										local function GetItemIconFromModels2(itemName)
											if not ItemsListModels or not itemName then
												return nil
											end
											local model = ItemsListModels:FindFirstChild(itemName)
											if not model then
												for _, child in ipairs(ItemsListModels:GetChildren()) do
													if string.lower(child.Name):find(string.lower(itemName)) then
														model = child
														break
													end
												end
											end
											if model then
												for _, desc in ipairs(model:GetDescendants()) do
													if desc:IsA("ImageLabel") or desc:IsA("ImageButton") then
														if desc.Image and desc.Image ~= "" then
															return desc.Image
														end
													elseif desc:IsA("Decal") or desc:IsA("Texture") then
														if desc.Texture and desc.Texture ~= "" then
															return desc.Texture
														end
													elseif
														desc:IsA("StringValue")
														and string.find(string.lower(desc.Name), "icon")
													then
														return desc.Value
													end
												end
											end
											return nil
										end
										local function GetPlayerEquipmentImages2(player)
											local icons = {}
											if not player then
												return icons
											end
											local RSPlayers = RS:FindFirstChild("Players")
											if not RSPlayers then
												return icons
											end
											local pdata = RSPlayers:FindFirstChild(player.Name)
											if not pdata then
												return icons
											end
											local inventory = pdata:FindFirstChild("Inventory")
											if not inventory then
												return icons
											end
											for _, item in ipairs(inventory:GetChildren()) do
												local itemName = item.Name
												if allowedItems2[itemName] then
													local icon = GetItemIconFromModels2(itemName)
													if icon then
														table.insert(icons, icon)
													end
													if #icons >= 7 then
														break
													end
												end
											end
											return icons
										end
										RunService.Heartbeat:Connect(function()
											local plr = closestplr()
											local imgs = plr and GetPlayerEquipmentImages2(plr) or {}
											for i = 1, 7 do
												slotImages[i].Image = imgs[i] or "rbxassetid://0"
											end
										end)
										Menu.Objects:List(nil, Armor.Objects["Holder"], nil, nil, nil)
									end
									--
									function Menu:Target()
										Target = {
											Objects = {},
											Items = {},
											Refresh = 5,
											State = true,
											Tick = tick(),
											Window = self,
										}

										local Players = game:GetService("Players")
										local RunService = game:GetService("RunService")
										local UserInputService = game:GetService("UserInputService")
										local LocalPlayer = Players.LocalPlayer
										local Camera = workspace.CurrentCamera
										local ReplicatedStorage = game:GetService("ReplicatedStorage")

										Target.Objects["Outline"], Target.Objects["Frame"] = Menu.Objects:Outline(
											Vector2.new(0, 0.5),
											Target.Window.Objects["Screen"],
											UDim2.new(0.57, 0, 0.05, 0),
											UDim2.new(0, 220, 0, 300),
											"Light Background",
											"Dark Inline",
											nil,
											1000
										)
										Target.Objects["Outline"].AnchorPoint = Vector2.new(0.5, 0)

										local frame = Target.Objects["Outline"]
										local dragging = false
										local dragStart, startPos
										local targetPos = frame.Position

										local function lerp(a, b, t)
											return a + (b - a) * t
										end

										RunService.RenderStepped:Connect(function()
											frame.Position = UDim2.new(
												lerp(frame.Position.X.Scale, targetPos.X.Scale, 0.2),
												lerp(frame.Position.X.Offset, targetPos.X.Offset, 0.2),
												lerp(frame.Position.Y.Scale, targetPos.Y.Scale, 0.2),
												lerp(frame.Position.Y.Offset, targetPos.Y.Offset, 0.2)
											)
										end)

										frame.InputBegan:Connect(function(input)
											if input.UserInputType == Enum.UserInputType.MouseButton1 then
												dragging = true
												dragStart = input.Position
												startPos = frame.Position
											end
										end)

										frame.InputEnded:Connect(function(input)
											if input.UserInputType == Enum.UserInputType.MouseButton1 then
												dragging = false
											end
										end)

										UserInputService.InputChanged:Connect(function(input)
											if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
												local delta = input.Position - dragStart
												targetPos = UDim2.new(
													startPos.X.Scale,
													startPos.X.Offset + delta.X,
													startPos.Y.Scale,
													startPos.Y.Offset + delta.Y
												)
											end
										end)

										Target.Objects["Screen_Glow"] = Menu.Objects:Create("ImageLabel", {
											BackgroundTransparency = 1,
											BorderSizePixel = 0,
											Parent = Target.Objects["Outline"],
											Position = UDim2.new(0.5, 0, 0.5, 0),
											Size = UDim2.new(1, 48, 1, 48),
											ImageColor3 = Color3.fromRGB(80, 80, 80),
											ScaleType = "Slice",
											ZIndex = -1,
											SliceCenter = Rect.new(49, 49, 450, 450),
											ImageTransparency = 0.6,
											AnchorPoint = Vector2.new(0.5, 0.5),
											ImageColor3 = "Accent",
											Image = "rbxassetid://6015897843",
										})

										Target.Objects["Gradient"] = Menu.Objects:Holder(
											nil,
											Target.Objects["Frame"],
											UDim2.new(0, 1, 0, 1),
											UDim2.new(1, -2, 0, 2)
										)

										Target.Objects["Gradient_Line1"] = Menu.Objects:Frame(
											nil,
											"Accent",
											nil,
											nil,
											nil,
											0,
											Target.Objects["Gradient"],
											nil,
											UDim2.new(1, 0, 0, 2),
											nil,
											1000
										)
										Target.Objects["Gradient_Line2"] = Menu.Objects:Frame(
											nil,
											"Outline",
											0.45,
											nil,
											nil,
											0,
											Target.Objects["Gradient"],
											UDim2.new(0, 0, 0, 1),
											UDim2.new(1, 0, 0, 1),
											nil,
											1000
										)

										Target.Objects["Text_Outline"] = Menu.Objects:Text(
											true,
											Vector2.new(0, 0),
											Target.Objects["Frame"],
											UDim2.new(0, 0, 0, 5),
											UDim2.new(1, 0, 0, 15),
											"",
											"Outline",
											"Center",
											1000
										)
										Target.Objects["Text"] = Menu.Objects:Text(
											true,
											nil,
											Target.Objects["Text_Outline"],
											UDim2.new(0, -1, 0, -1),
											UDim2.new(1, 0, 1, 0),
											"Target",
											"Light Text",
											"Center",
											1000
										)

										Target.Objects["Holder_Outline"], Target.Objects["Holder_Frame"] =
											Menu.Objects:Outline(
												nil,
												Target.Objects["Frame"],
												UDim2.new(0, 2, 0, 22),
												UDim2.new(1, -4, 1, -24),
												"Lighter Background",
												nil,
												nil,
												1000
											)

										Target.Objects["Holder"] = Menu.Objects:Holder(
											nil,
											Target.Objects["Holder_Frame"],
											UDim2.new(0, 4, 0, 2),
											UDim2.new(1, -8, 1, -4)
										)

										Menu.Objects:List(nil, Target.Objects["Holder"], nil, nil, nil)
									end
								end
								--
								function Menu:Page(Properties)
									local Properties = (Properties or {})
									local Page = {
										Objects = {},
										Sections = {
											left = {},
											right = (not self.Single and {} or nil),
										},
										--
										State = false,
										--
										Window = self,
										--
										Name = (Utils:Property(Properties, "Name", "Text") or "Page"),
									}
									--
									do -- Objects
										Page.Objects["Outline"], Page.Objects["Outline_Frame"] = Menu.Objects:Outline(
											nil,
											Page.Window.Objects["Buttons"],
											nil,
											UDim2.new(0, 1, 1, 0),
											"Dark Background",
											"Light Inline"
										)
										--
										Menu.Objects:Gradient(
											Color3.fromRGB(0, 0, 0),
											-90,
											0.4,
											Page.Objects["Outline_Frame"]
										)
										--
										Page.Objects["Text"] = Menu.Objects:Text(
											true,
											nil,
											Page.Objects["Outline_Frame"],
											nil,
											UDim2.new(1, 0, 1, 0),
											Page.Name,
											"Dark Text",
											"Center",
											nil,
											nil
										)
										--
										Page.Objects["Frame"] = Menu.Objects:Holder(
											nil,
											Page.Window.Objects["Frame"],
											UDim2.new(0, 14, 0, 14),
											UDim2.new(1, -28, 1, -28),
											false
										)
										Page.Objects["Frame"].BackgroundTransparency = 1
										--
										Page.Objects["left"] = Menu.Objects:Holder(
											nil,
											Page.Objects["Frame"],
											UDim2.new(0, 0, 0, 0),
											(Page.Window.Single and UDim2.new(1, 0, 1, 0) or UDim2.new(0.5, -6, 1, 0))
										)
										--
										if not Page.Window.Single then
											Page.Objects["right"] = Menu.Objects:Holder(
												Vector2.new(1, 0),
												Page.Objects["Frame"],
												UDim2.new(1, 0, 0, 0),
												UDim2.new(0.5, -6, 1, 0)
											)
										end
										--
										Menu.Objects:List(UDim.new(0, 14), Page.Objects["left"], "Vertical", "Left", "Top")
										Menu.Objects:List(UDim.new(0, 14), Page.Objects["right"], "Vertical", "Left", "Top")
										--
										Page.Objects["Button"] = Menu.Objects:Button(Page.Objects["Outline"])
									end
									--
									do -- Functions
										function Page:Initialise(Size)
											Page.Objects["Outline"].Size = UDim2.new(Size, 1, 1, 0)
											--
											local Totals = {}
											--
											for Index, Value in next, Page.Sections do
												Totals[Index] = {
													Total = 0,
													Objects = {},
												}
												--
												for Index2, Value2 in next, Value do
													if Value2.Size then
														Totals[Index].Total = (Totals[Index].Total + Value2.Size)
													else
														Totals[Index].Objects[#Totals[Index].Objects + 1] = Value2
													end
												end
											end
											--
											for Index, Value in next, Totals do
												if #Value.Objects > 0 then
													for Index2, Value2 in next, Value.Objects do
														Value2.Size = ((100 - Value.Total) / #Value.Objects)
													end
												end
											end
											--
											for Index, Value in next, Page.Sections do
												local Frame = Page.Objects[Index]
												local Total, Size = 0, (Frame.AbsoluteSize.Y - ((#Value - 1) * 14))
												--
												for Index2, Value2 in next, Value do
													local Size2 = math.floor(Size * (Value2.Size / 100))
													--
													Total = (Total + Size2)
													--
													Value2:Initialise(Size2)
												end
											end
										end
										--
										function Page:Set(State)
											Page.State = State
											--
											Menu.Objects:Update(
												Page.Objects["Text"],
												{ TextColor3 = (Page.State and "Accent" or "Dark Text") }
											)
											Menu.Objects:Update(Page.Objects["Frame"], { Visible = Page.State })
										end
									end
									--
									do -- Connections
										Utils:Connect(Page.Objects["Button"].MouseEnter, function()
											Menu.Objects:Update(Page.Objects["Text"], { TextColor3 = "Accent" })
										end)
										--
										Utils:Connect(Page.Objects["Button"].MouseLeave, function()
											Menu.Objects:Update(
												Page.Objects["Text"],
												{ TextColor3 = (Page.State and "Accent" or "Dark Text") }
											)
										end)
										--
										Utils:Connect(Page.Objects["Button"].MouseButton1Down, function()
											if not Page.State then
												Page:Set(true)
												--
												for Index, Value in next, Page.Window.Pages do
													if Value ~= Page and Value.State then
														Value:Set(false)
													end
												end
											end
										end)
									end
									--
									do -- Setup
										Page.Window.Pages[#Page.Window.Pages + 1] = Page
										--
										return setmetatable(Page, {
											__index = Menu.Pages,
										})
									end
								end
								--
								function Menu.Pages:Section(Properties)
									local Properties = (Properties or {})
									local Section = {
										Objects = {},
										Content = {},
										--
										Window = self.Window,
										Page = self,
										--
										Name = (Utils:Property(Properties, "Name", "Text") or "Section"),
										Size = (Utils:Property(Properties, "Size", "Height", "Percent")),
										Side = string.lower(
											(Utils:Property(Properties, "Side", "Direction", "Dir") or "Left")
										),
									}
									--
									do -- Objects
										local Parent
										local FrameSize

										if Section.Side == "middle" then
											Parent = Section.Page.Objects.Frame
											FrameSize = UDim2.new(1, 0, 0, 250)
										else
											Parent = Section.Page.Objects[Section.Side]
											FrameSize = UDim2.new(1, 0, 0, 250)
										end

										Section.Objects["Frame"] = Menu.Objects:Outline(
											nil,
											Parent,
											nil,
											FrameSize,
											nil,
											"Light Background",
											"Light Inline"
										)

										Section.Objects["Content"] = Menu.Objects:Scroll(
											Section.Objects["Frame"],
											true,
											UDim.new(0, 0),
											UDim.new(0, 11),
											UDim.new(0, 11)
										)

										Section.Objects["Line"] = Menu.Objects:Frame(
											nil,
											"Light Background",
											nil,
											nil,
											nil,
											0,
											Section.Objects["Frame"],
											UDim2.new(0, 9, 0, -2),
											UDim2.new(0, 0, 0, 2)
										)

										Section.Objects["Text"] = Menu.Objects:Text(
											true,
											Vector2.new(0, 0.5),
											Section.Objects["Frame"],
											UDim2.new(0, 12, 0, 0),
											UDim2.new(1, -20, 0, 20),
											Section.Name,
											"Light Text"
										)

										Section.Objects["Open"] = Menu.Objects:Holder(
											nil,
											Section.Objects["Frame"],
											UDim2.new(0, 0, 0, 1),
											UDim2.new(1, 0, 1, -2),
											nil
										)
									end
									--
									do -- Functions
										function Section:Initialise(Size)
											Section.Objects["Frame"].Size = UDim2.new(1, 0, 0, Size)
											Section.Objects["Line"].Size =
												UDim2.new(0, (Section.Objects["Text"].TextBounds.X + 6), 0, 2)
											--
											for Content, Value in next, Section.Content do
												if not Content.NoCallback then
													Utils:Call(Content.Set, Content, Content.State, Content.Alpha)
												end
											end
										end
									end
									--
									do -- Setup
										if Section.Side == "middle" then
											Section.Page.Sections.Middle = Section.Page.Sections.Middle or {}
											Section.Page.Sections.Middle[#Section.Page.Sections.Middle + 1] = Section
										else
											Section.Page.Sections[Section.Side][#Section.Page.Sections[Section.Side] + 1] =
												Section
										end

										return setmetatable(Section, {
											__index = Menu.Sections,
										})
									end
								end

								--
								function Menu:SettingsPage()
									local Self = self
									--
									local Settings = Self:Page({ Name = "Settings" })
									do
										local Theming, Extra, Presets =
											Settings:Section({ Name = "Theming" }),
											Settings:Section({ Name = "Extra" }),
											Settings:Section({ Name = "Presets", Side = "Right" })
										--
										do -- Colors
											for _, Theme in pairs({
												"Accent",
												"Lighter Background",
												"Light Background",
												"Dark Background",
												"Outline",
												"Lighter Inline",
												"Light Inline",
												"Dark Inline",
												"Light Text",
												"Dark Text",
											}) do
												Theming:Label({ Name = Theme }):Picker({
													State = Menu.Themes[Theme],
													Flag = Theme,
													Callback = function(State)
														Menu.Presets:Update(Theme, State)
													end,
												})
											end
											--
											do
												local ThemeList = {}
												for Index, _ in pairs(Menu.Presets.Themes) do
													ThemeList[Index] = Index
												end
												--
												Theming:Label({ Name = "Menu Theme", Inlayed = true })
												Theming:SearchList({
													PlaceHolder = "_",
													Options = ThemeList,
													State = "Niggahack",
													Flag = "Menu Theme",
													Shown = 8,
													Callback = function(State)
														if State then
															Menu.Presets:Set(Env.Menu, State)
														end
													end,
												})
											end
										end
										--
										do -- Extra
											Extra:Label({ Name = "Hide Menu" }):Bind({
												State = Enum.KeyCode.RightShift,
												Mode = "Off Toggle",
												Disabled = true,
												Flag = "UI Bind",
												Callback = function()
													Self:Switch(not Self.State)
												end,
											})
											--
											Extra:Button({
												Name = "Unload",
												Callback = function()
													Utils:Unload()
												end,
											})
											--
											Extra:Button({
												Name = "Reconnect",
												Callback = function()
													Services.TeleportService:Teleport(game.PlaceId, Client)
												end,
											})
											--
											Extra:Button({
												Name = "Copy Server Connect",
												Callback = function()
													setclipboard(
														[[game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, game.JobId, game:GetService("Players").LocalPlayer)]]
													)
												end,
											})
											--
											Extra:Button({
												Name = "Prompt Discord Invite",
												Callback = function() end,
											})
											--
											Extra:Button({
												Name = "Copy Discord Invite Code",
												Callback = function()
													setclipboard(Env.Invite)
												end,
											})
											--
											Extra:Toggle({
												Name = "Binds",
												Flag = "Binds Widget",
												Default = true,
												Callback = function(State)
													Flags["Binds"]:Set(State)
												end,
											})
											--
										end
										--
										do -- Presets
											Presets:Label({ Name = "Configs", Inlayed = true, Wide = true })
											Presets:SearchList({
												PlaceHolder = "_",
												Flag = "Preset Configurations",
												Wide = true,
												Shown = 8,
												Options = {},
												Callback = function()
													Utils.Configs:Update()
												end,
											})
											--
											Presets:Box({
												Wide = true,
												Flag = "Preset Name",
												Callback = function(State)
													local Normal = Utils.String:Normalise(State)
													--
													if Normal ~= State then
														Flags["Preset Name"]:Set(Normal)
													else
														Flags["Present Create Holder"]:Set(
															State ~= "" and State ~= " " and #State > 2
														)
													end
													--
													Utils.Configs:Update()
												end,
											})
											--
											Presets:Holder({
												Return = function(Self)
													Self:Button({
														Name = "Create",
														Wide = true,
														Callback = function()
															local Name = Flags["Preset Name"]:Get()
															if
																Name == ""
																or isfile(
																	string.format("%s/Configs/%s.cfg", Menu.FileName, Name)
																)
															then
																return
															end
															--
															writefile(
																string.format("%s/Configs/%s.cfg", Menu.FileName, Name),
																""
															)
															--
															Menu:Toast({
																Text = string.format(
																	"Successfully created config: %s.",
																	Name
																),
															})
															--
															Utils.Configs:Update()
														end,
													})
												end,
												Flag = "Present Create Holder",
											})
											--
											Presets:Button({
												Name = "Load",
												Wide = true,
												Callback = function()
													local Selected = Flags["Preset Configurations"]:Get()
													--
													if Selected then
														Utils.Configs:Set(
															readfile(
																string.format("%s/Configs/%s.cfg", Menu.FileName, Selected)
															)
														)
														--
														Menu:Toast({ Text = string.format("Loaded config: %s.", Selected) })
													end
												end,
											})
											--
											Presets:Button({
												Name = "Save",
												Wide = true,
												Callback = function()
													local Selected = Flags["Preset Configurations"]:Get()
													--
													if Selected then
														writefile(
															string.format("%s/Configs/%s.cfg", Menu.FileName, Selected),
															Utils.Configs:Get()
														)
														--
														Utils.Configs:Update()
														--
														Menu:Toast({
															Text = string.format(
																"Successfully saved config data to %s.",
																Selected
															),
														})
													end
												end,
											})
											--
											Presets:Button({
												Name = "Delete",
												Wide = true,
												Callback = function()
													local Selected = Flags["Preset Configurations"]:Get()
													--
													if Selected then
														delfile(string.format("%s/Configs/%s.cfg", Menu.FileName, Selected))
														--
														Utils.Configs:Update()
														--
														Menu:Toast({
															Text = string.format(
																"Successfully deleted config: %s.",
																Selected
															),
														})
													end
												end,
											})
											--
											Presets:Button({
												Name = "Export to Clipboard",
												Wide = true,
												Callback = function()
													setclipboard(Utils.Configs:Get())
													--
													Menu:Toast({ Text = "Successfully exported data to the clipboard." })
												end,
											})
											--
											Presets:Button({
												Name = "Refresh",
												Wide = true,
												Callback = function()
													Utils.Configs:Update()
												end,
											})
											--
											Utils.Configs:Update()
										end
									end
								end
							end
							--
							do -- Content
								function Menu.Sections:Holder(Properties)
									local Properties = (Properties or {})
									local Content = {
										Objects = {},
										--
										Window = self.Window,
										Page = self.Page,
										Section = (self.Section or self),
										--
										Flag = (Utils:Property(Properties, "Flag", "Pointer")),
										Small = (Utils:Property(Properties, "Decreased", "Minified", "Smaller", "Small")),
										Callback = (Utils:Property(Properties, "Callback", "Function", "Func", "Return")),
									}
									--
									do -- Objects
										Content.Objects["Holder"] = Menu.Objects:Holder(
											nil,
											self.Objects["Content"],
											nil,
											UDim2.new(1, (Content.Small and -12 or 0), 0, 0),
											true
										)
										--
										Menu.Objects:List(nil, Content.Objects["Holder"], nil, "Center")
									end
									--
									do -- Functions
										function Content:Set(State)
											Content.Objects["Holder"].Visible = State
										end
									end
									--
									do -- Setup
										Content.Objects["Holder"].AutomaticSize = "Y"
										Content.Objects["Content"] = Content.Objects["Holder"]
										--
										if Content.Flag then
											Flags[Content.Flag] = Content
										end
										--
										local Content = setmetatable(Content, {
											__index = Menu.Sections,
										})
										--
										Content.Callback(Content)
										--
										return Content
									end
								end
								--
								function Menu.Sections:Label(Properties)
									local Properties = (Properties or {})
									local Content = {
										Objects = {},
										--
										Window = self.Window,
										Page = self.Page,
										Section = (self.Section or self),
										--
										Offset = 0,
										--
										Name = (Utils:Property(Properties, "Name", "Text") or "Content"),
										Center = (Utils:Property(Properties, "Center", "Middle") or false),
										Inlay = (Utils:Property(Properties, "Inlayed", "Inlay") or false),
										Wide = (Utils:Property(Properties, "Wide", "Wider") or false),
										Tooltip = (Utils:Property(Properties, "Tooltip", "Tip") or nil),
									}
									--
									if Content.Tooltip then
										Content.Name = Content.Name .. ' <font color="rgb(180, 180, 180)">(?)</font>'
									end
									--
									do -- Objects
										Content.Objects["Holder"] = Menu.Objects:Holder(
											nil,
											self.Objects["Content"],
											nil,
											UDim2.new(1, 0, 0, (Content.Inlay and 12 or 16))
										)
										--
										Content.Objects["Text"] = Menu.Objects:Text(
											true,
											nil,
											Content.Objects["Holder"],
											UDim2.new(0, (Content.Wide and 25 or 37), 0, (Content.Inlay and 1 or -1)),
											UDim2.new(1, -37, 1, 0),
											Content.Name,
											"Light Text",
											(Content.Center and "Center")
										)
									end
									--
									do -- Functions
										function Content:Picker(Properties)
											return Menu.Sections.Addons:Picker(Content, Properties)
										end
										--
										function Content:Bind(Properties)
											return Menu.Sections.Addons:Bind(Content, Properties)
										end
									end
									--
									do -- Connections
										Utils:Connect(Content.Objects["Holder"].MouseEnter, function()
											Menu.Objects:Update(Content.Objects["Text"], { TextColor3 = "Accent" })
										end)
										--
										Utils:Connect(Content.Objects["Holder"].MouseLeave, function()
											Menu.Objects:Update(Content.Objects["Text"], { TextColor3 = "Light Text" })
										end)
										--
										do -- Tooltip
											if Content.Tooltip then
												Utils:Connect(Content.Objects["Holder"].InputBegan, function(Input)
													if Input.UserInputType.Name == "MouseMovement" then
														Content.Window.Objects["ToolTip_Text"].Text = Content.Tooltip
														Content.Window.Objects["Tooltip_Outline"].Size = UDim2.new(
															0,
															Content.Window.Objects["ToolTip_Text"].TextBounds.X + 9,
															0,
															Content.Window.Objects["ToolTip_Text"].TextBounds.Y + 9
														)

														Content.Window.Objects["Tooltip_Outline"].Position =
															UDim2.new(0, Input.Position.X + 5, 0, Input.Position.Y + 35)
													end
												end)
												--
												Utils:Connect(Content.Objects["Holder"].InputChanged, function(Input)
													if Input.UserInputType.Name == "MouseMovement" then
														Content.Window.Objects["Tooltip_Outline"].Position =
															UDim2.new(0, Input.Position.X + 5, 0, Input.Position.Y + 35)
													end
												end)
												--
												Utils:Connect(Content.Objects["Holder"].InputEnded, function(Input)
													if Input.UserInputType.Name == "MouseMovement" then
														Content.Window.Objects["ToolTip_Text"].Text = ""
														Content.Window.Objects["Tooltip_Outline"].Position = UDim2.new(2)
													end
												end)
											end
										end
									end
									--
									do -- Setup
										if Content.Inlay then
											Content.Offset = 2
										end
										--
										return Content
									end
								end
								--
								function Menu.Sections:Button(Properties)
									local Properties = (Properties or {})
									local Content = {
										Objects = {},
										--
										Window = self.Window,
										Page = self.Page,
										Section = (self.Section or self),
										--
										Name = (Utils:Property(Properties, "Name", "Text") or "Content"),
										Wide = (Utils:Property(Properties, "Wide", "Wider") or false),
										Callback = (Utils:Property(Properties, "Callback", "Function", "Func", "Return")),
									}
									--
									do -- Objects
										Content.Objects["Holder"] =
											Menu.Objects:Holder(nil, self.Objects["Content"], nil, UDim2.new(1, 0, 0, 23))
										--
										Content.Objects["Outline"], Content.Objects["Frame"] = Menu.Objects:Outline(
											nil,
											Content.Objects["Holder"],
											UDim2.new(0, (Content.Wide and 24 or 36), 0, 4),
											UDim2.new(1, (Content.Wide and -49 or -69), 0, 18),
											"Lighter Background"
										)
										--
										Menu.Objects:Gradient(Color3.fromRGB(0, 0, 0), -90, 0.7, Content.Objects["Frame"])
										--
										Content.Objects["Text"] = Menu.Objects:Text(
											true,
											nil,
											Content.Objects["Frame"],
											UDim2.new(0, 0, 0, 0),
											UDim2.new(1, 0, 1, 0),
											Content.Name,
											"Light Text",
											"Center"
										)
										--
										Content.Objects["Button"] = Menu.Objects:Button(Content.Objects["Holder"])
									end
									--
									do -- Connections
										Utils:Connect(Content.Objects["Button"].MouseEnter, function()
											Menu.Objects:Update(Content.Objects["Text"], { TextColor3 = "Accent" })
											--
											Menu.Objects:Update(Content.Objects["Outline"], { BorderColor3 = "Accent" })
										end)
										--
										Utils:Connect(Content.Objects["Button"].MouseLeave, function()
											Menu.Objects:Update(Content.Objects["Text"], { TextColor3 = "Light Text" })
											--
											Menu.Objects:Update(Content.Objects["Outline"], { BorderColor3 = "Outline" })
										end)
										--
										Utils:Connect(Content.Objects["Button"].MouseButton1Down, function()
											Utils:Thread(function()
												Content.Objects["Frame"].BackgroundTransparency = 0.2
												--
												task.wait(0.075)
												--
												Content.Objects["Frame"].BackgroundTransparency = 0
											end)
											--
											Utils:Call(Content.Callback)
										end)
									end
									--
									do -- Setup
										return Content
									end
								end
								--
								function Menu.Sections:Box(Properties)
									local Properties = (Properties or {})
									local Content = {
										Objects = {},
										--
										Window = self.Window,
										Page = self.Page,
										Section = (self.Section or self),
										--
										State = (Utils:Property(Properties, "State", "Current", "Default", "Def") or ""),
										Holder = (Utils:Property(
											Properties,
											"Name",
											"Text",
											"Holder",
											"PlaceHolder",
											"PlaceHolderText"
										) or ""),
										Confirm = (Utils:Property(Properties, "Confirmation", "Confirm") or false),
										Wide = (Utils:Property(Properties, "Wide", "Wider") or false),
										Flag = (Utils:Property(Properties, "Flag", "Pointer")),
										Callback = (Utils:Property(Properties, "Callback", "Function", "Func", "Return")),
									}
									--
									do -- Objects
										Content.Objects["Holder"] =
											Menu.Objects:Holder(nil, self.Objects["Content"], nil, UDim2.new(1, 0, 0, 25))
										--
										Content.Objects["Outline"], Content.Objects["Outline_Frame"] = Menu.Objects:Outline(
											nil,
											Content.Objects["Holder"],
											UDim2.new(0, (Content.Wide and 24 or 36), 0, 4),
											UDim2.new(1, (Content.Wide and -49 or -69), 0, 20),
											"Outline"
										)
										--
										Content.Objects["Frame"] = Menu.Objects:Frame(
											nil,
											"Lighter Background",
											nil,
											nil,
											nil,
											0,
											Content.Objects["Outline_Frame"],
											UDim2.new(0, 1, 0, 1),
											UDim2.new(1, -2, 1, -2)
										)
										--
										Content.Objects["Value"] = Menu.Objects:Box(
											nil,
											Content.Objects["Frame"],
											UDim2.new(0, 6, 0, 0),
											UDim2.new(1, -12, 1, 0),
											"",
											Content.Holder,
											"Light Text",
											"Left",
											nil,
											"AtEnd"
										)
										--
										Content.Objects["Button"] = Menu.Objects:Button(Content.Objects["Holder"])
									end
									--
									do -- Functions
										function Content:Set(State)
											Content.State = State
											--
											Content.Objects["Value"].Text = Content.State
											--
											Utils:Call(Content.Callback, Content.State)
										end
										--
										function Content:Get(Raw)
											return Content.State
										end
									end
									--
									do -- Connections
										Utils:Connect(Content.Objects["Value"]:GetPropertyChangedSignal("Text"), function()
											if not Content.Confirm then
												Content:Set(Content.Objects["Value"].Text)
											end
										end)
										--
										Utils:Connect(Content.Objects["Value"].Focused, function()
											Menu.Objects:Update(Content.Objects["Outline"], {
												BackgroundColor3 = "Accent",
											})
										end)
										--
										Utils:Connect(Content.Objects["Value"].FocusLost, function()
											if Content.Confirm then
												Content:Set(Content.Objects["Value"].Text)
											end
											--
											Menu.Objects:Update(Content.Objects["Outline"], {
												BackgroundColor3 = "Light Inline",
											})
										end)
										--
										Utils:Connect(Content.Objects["Button"].MouseEnter, function()
											Menu.Objects:Update(Content.Objects["Value"], { PlaceholderColor3 = "Accent" })
										end)
										--
										Utils:Connect(Content.Objects["Button"].MouseLeave, function()
											Menu.Objects:Update(
												Content.Objects["Value"],
												{ PlaceholderColor3 = "Light Text" }
											)
										end)
										--
										Utils:Connect(Content.Objects["Button"].MouseButton1Down, function()
											Content.Objects["Value"]:CaptureFocus()
										end)
									end
									--
									do -- Setup
										Content.Section.Content[Content] = true
										--
										if Content.Flag then
											Flags[Content.Flag] = Content
										end
										--
										return Content
									end
								end
								--
								function Menu.Sections:List(Properties)
									local Properties = (Properties or {})
									local Content = {
										Objects = {},
										Items = {},
										--
										Window = self.Window,
										Page = self.Page,
										Section = (self.Section or self),
										--
										State = (Utils:Property(Properties, "State", "Current", "Default", "Def") or 1),
										Shown = (Utils:Property(Properties, "Shown", "Listed", "Viewed") or 10),
										Maximum = (Utils:Property(Properties, "Maximum", "Max", "Most", "Maximal") or 1),
										Minimum = (Utils:Property(Properties, "Minimum", "Min", "Least", "Minimal") or 1),
										Options = (
											Utils:Property(Properties, "Options", "List") or { "1", "2", "3", "4", "5" }
										),
										Selected = (Utils:Property(Properties, "Global", "Selected", "Select")),
										Wide = (Utils:Property(Properties, "Wide", "Wider") or false),
										Flag = (Utils:Property(Properties, "Flag", "Pointer")),
										Callback = (Utils:Property(Properties, "Callback", "Function", "Func", "Return")),
										NoCallback = (Utils:Property(
											Properties,
											"NoCallback",
											"DontCallback",
											"IgnoreCallback",
											"Ignore"
										) or false),
									}
									--
									do -- Objects
										Content.Objects["Holder"] = Menu.Objects:Holder(
											nil,
											self.Objects["Content"],
											nil,
											UDim2.new(1, 0, 0, ((Content.Shown * 18) + 6 + 2))
										)
										--
										Content.Objects["Outline"], Content.Objects["Frame"] = Menu.Objects:Outline(
											nil,
											Content.Objects["Holder"],
											UDim2.new(0, (Content.Wide and 24 or 36), 0, 2),
											UDim2.new(1, (Content.Wide and -49 or -69), 0, (Content.Shown * 18) + 6),
											"Lighter Background"
										)
										--
										Menu.Objects:Gradient(Color3.fromRGB(0, 0, 0), -90, 0.7, Content.Objects["Frame"])
										--
										Content.Objects["Content"] = Menu.Objects:Scroll(Content.Objects["Frame"])
									end
									--
									do -- Functions
										function Content:Set(State)
											Content.State = State
											--
											if typeof(Content.State) == "table" then
												for key, item in next, Content.Items do
													item:Set(Content.State[key] == true)
												end
											else
												for key, item in next, Content.Items do
													if key == Content.State then
														item:Set(true)
													elseif item.State then
														item:Set(false)
													end
												end
											end
											--
											Utils:Call(Content.Callback, Content.State)
										end
										--
										function Content:Get(Raw)
											return Content.State
										end
										--
										function Content:Insert(Value)
											if not Content.Items[Value] then
												local Item = {
													Connections = {},
													Objects = {},
													--
													State = false,
												}
												--
												do -- Objects
													Item.Objects["Holder"] = Menu.Objects:Holder(
														nil,
														Content.Objects["Content"],
														nil,
														UDim2.new(1, 0, 0, 18)
													)
													Item.Objects["Text"] = Menu.Objects:Text(
														false,
														nil,
														Item.Objects["Holder"],
														UDim2.new(0, 12, 0, 0),
														UDim2.new(1, -24, 1, 0),
														Value,
														"Light Text"
													)
													--
													Item.Objects["Button"] = Menu.Objects:Button(Item.Objects["Holder"])
												end
												--
												do -- Functions
													function Item:Remove()
														for i, connection in next, Item.Connections do
															connection:Disconnect()
														end
														--
														for i, object in next, Item.Objects do
															object:Remove()
														end
														--
														Item.Connections = nil
														Item.Objects = nil
														Item.State = nil
														Item = nil
													end
													--
													function Item:Show(visible)
														Item.Objects["Holder"].Visible = visible
													end
													--
													function Item:Set(state)
														Item.State = state
														--
														Menu.Objects:Update(Item.Objects["Text"], {
															TextColor3 = (Item.State and "Accent" or "Light Text"),
														})
													end
												end
												--
												do -- Connections
													Item.Connections[#Item.Connections + 1] = Item.Objects["Button"].MouseButton1Down:Connect(
														function()
															-- Directly set the state to the actual value instead of a table index
															Content:Set(Value)
														end
													)
													--
													Utils:Connect(Item.Objects["Button"].MouseEnter, function()
														Menu.Objects:Update(Item.Objects["Text"], { TextColor3 = "Accent" })
													end)
													--
													Utils:Connect(Item.Objects["Button"].MouseLeave, function()
														Menu.Objects:Update(
															Item.Objects["Text"],
															{ TextColor3 = (Item.State and "Accent" or "Light Text") }
														)
													end)
												end
												--
												Content.Items[Value] = Item
											end
										end
										--
										function Content:Remove(Value)
											local Item = Content.Items[Value]
											--
											if Item then
												Item:Remove()
											end
										end
										--
										function Content:Refresh(Options, Selected)
											Content.Options = Options
											Content.Selected = (Selected or Content.Options)
											--
											for key, item in next, Content.Items do
												if not Content.Options[key] then
													item:Remove()
													Content.Items[key] = nil
												end
											end
											--
											for key, _ in next, Content.Options do
												if not Content.Items[key] then
													Content:Insert(key)
												end
											end
											--
											for key, _ in next, Content.Options do
												Content.Items[key]:Show(Content.Selected[key])
											end
										end
									end
									--
									do -- Connections
										Utils:Connect(Content.Objects["Content"].MouseEnter, function()
											Menu.Objects:Update(Content.Objects["Outline"], { BorderColor3 = "Accent" })
										end)
										--
										Utils:Connect(Content.Objects["Content"].MouseLeave, function()
											Menu.Objects:Update(Content.Objects["Outline"], { BorderColor3 = "Outline" })
										end)
									end
									--
									do -- Setup
										Content.Section.Content[Content] = true
										--
										local OptionsTable = {}
										--
										for _, Value in next, Content.Options do
											OptionsTable[Value] = true
										end
										--
										Content:Refresh(OptionsTable)
										--
										if Content.Flag then
											Flags[Content.Flag] = Content
										end
										--
										return Content
									end
								end
								--
								function Menu.Sections:Toggle(Properties)
									local Properties = (Properties or {})
									local Content = {
										Objects = {},
										--
										Window = self.Window,
										Page = self.Page,
										Section = (self.Section or self),
										--
										Offset = 0,
										--
										Name = (Utils:Property(Properties, "Name", "Text") or "Content"),
										State = (Utils:Property(Properties, "State", "Current", "Default", "Def") or false),
										Flag = (Utils:Property(Properties, "Flag", "Pointer")),
										Callback = (Utils:Property(Properties, "Callback", "Function", "Func", "Return")),
										Tooltip = (Utils:Property(Properties, "Tooltip", "Tip") or nil),
									}
									--
									if Content.Tooltip then
										Content.Name = Content.Name .. ' <font color="rgb(180, 180, 180)">(?)</font>'
									end
									--
									do -- Objects
										Content.Objects["Holder"] =
											Menu.Objects:Holder(nil, self.Objects["Content"], nil, UDim2.new(1, 0, 0, 16))
										--
										Content.Objects["Frame"] = Menu.Objects:Outline(
											nil,
											Content.Objects["Holder"],
											UDim2.new(0, 16, 0, 4),
											UDim2.new(0, 8, 0, 8),
											nil,
											"Light Inline"
										)
										--
										Content.Objects["Text"] = Menu.Objects:Text(
											true,
											nil,
											Content.Objects["Holder"],
											UDim2.new(0, 37, 0, -1),
											UDim2.new(1, -37, 1, 0),
											Content.Name,
											"Light Text"
										)
										--
										Content.Objects["Button"] = Menu.Objects:Button(Content.Objects["Holder"])
										--
										Menu.Objects:Gradient(
											Color3.fromRGB(85, 85, 85),
											-90,
											nil,
											Content.Objects["Button"]
										)
									end
									--
									do -- Functions
										function Content:Set(State)
											Content.State = State
											--
											Menu.Objects:Update(Content.Objects["Frame"], {
												BackgroundColor3 = (Content.State and "Accent" or "Lighter Inline"),
											})
											--
											Utils:Call(Content.Callback, Content.State)
										end
										--
										function Content:Get(Raw)
											return Content.State
										end
										--
										function Content:Picker(Properties)
											return Menu.Sections.Addons:Picker(Content, Properties)
										end
										--
										function Content:Bind(Properties)
											return Menu.Sections.Addons:Bind(Content, Properties)
										end
									end
									--
									do -- Connections
										Utils:Connect(Content.Objects["Button"].MouseEnter, function()
											Menu.Objects:Update(Content.Objects["Text"], { TextColor3 = "Accent" })
											--
											Menu.Objects:Update(Content.Objects["Frame"], { BorderColor3 = "Accent" })
										end)
										--
										Utils:Connect(Content.Objects["Button"].MouseLeave, function()
											Menu.Objects:Update(Content.Objects["Text"], { TextColor3 = "Light Text" })
											--
											Menu.Objects:Update(Content.Objects["Frame"], { BorderColor3 = "Outline" })
										end)
										--
										Utils:Connect(Content.Objects["Button"].MouseButton1Down, function()
											Content:Set(not Content.State)
										end)
										--
										do -- Tooltip
											if Content.Tooltip then
												Utils:Connect(Content.Objects["Button"].InputBegan, function(Input)
													if Input.UserInputType.Name == "MouseMovement" then
														Content.Window.Objects["ToolTip_Text"].Text = Content.Tooltip
														Content.Window.Objects["Tooltip_Outline"].Size = UDim2.new(
															0,
															Content.Window.Objects["ToolTip_Text"].TextBounds.X + 9,
															0,
															Content.Window.Objects["ToolTip_Text"].TextBounds.Y + 9
														)

														Content.Window.Objects["Tooltip_Outline"].Position =
															UDim2.new(0, Input.Position.X + 5, 0, Input.Position.Y + 35)
													end
												end)
												--
												Utils:Connect(Content.Objects["Button"].InputChanged, function(Input)
													if Input.UserInputType.Name == "MouseMovement" then
														Content.Window.Objects["Tooltip_Outline"].Position =
															UDim2.new(0, Input.Position.X + 5, 0, Input.Position.Y + 35)
													end
												end)
												--
												Utils:Connect(Content.Objects["Button"].InputEnded, function(Input)
													if Input.UserInputType.Name == "MouseMovement" then
														Content.Window.Objects["ToolTip_Text"].Text = ""
														Content.Window.Objects["Tooltip_Outline"].Position = UDim2.new(2)
													end
												end)
											end
										end
									end
									--
									do -- Setup
										Content.Section.Content[Content] = true
										--
										if Content.Flag then
											Flags[Content.Flag] = Content
										end
										--
										return Content
									end
								end
								--
								function Menu.Sections:Slider(Properties)
									local Properties = (Properties or {})
									local Content = {
										Objects = {},
										--
										Current = "",
										Tick = tick(),
										--
										Window = self.Window,
										Page = self.Page,
										Section = (self.Section or self),
										--
										State = (Utils:Property(Properties, "State", "Current", "Default", "Def") or 50),
										Maximum = (Utils:Property(Properties, "Maximum", "Max", "Most", "Maximal") or 100),
										Minimum = (Utils:Property(Properties, "Minimum", "Min", "Least", "Minimal") or 0),
										Decimals = (1 / (Utils:Property(Properties, "Decimals", "Rounding", "Tick") or 1)),
										Ending = (Utils:Property(Properties, "Ending", "Suffix", "Sub") or ""),
										Flag = (Utils:Property(Properties, "Flag", "Pointer")),
										Callback = (Utils:Property(Properties, "Callback", "Function", "Func", "Return")),
									}
									--
									do -- Objects
										Content.Objects["Holder"] =
											Menu.Objects:Holder(nil, self.Objects["Content"], nil, UDim2.new(1, 0, 0, 18))
										--
										Content.Objects["Outline"] = Menu.Objects:Outline(
											nil,
											Content.Objects["Holder"],
											UDim2.new(0, 36, 0, 5),
											UDim2.new(1, -69, 0, 8),
											nil,
											"Lighter Inline"
										)
										--
										Content.Objects["Frame"] = Menu.Objects:Frame(
											nil,
											"Accent",
											nil,
											nil,
											nil,
											0,
											Content.Objects["Outline"],
											nil,
											UDim2.new(0, 0, 1, 0)
										)
										--
										Menu.Objects:Gradient(
											Color3.fromRGB(85, 85, 85),
											-90,
											nil,
											Content.Objects["Frame"]
										)
										--
										Content.Objects["Value"] = Menu.Objects:Box(
											nil,
											Content.Objects["Frame"],
											UDim2.new(1, 0, 0, 3),
											UDim2.new(0, 0, 1, 0),
											"",
											nil,
											"Light Text",
											"Center"
										)
										--
										Content.Objects["Button"] = Menu.Objects:Button(Content.Objects["Holder"])
									end
									--
									do -- Functions
										function Content:Set(State)
											Content.Last = Content.State
											--
											Content.State = Utils.Math:Normalise(
												State,
												Content.Decimals,
												Content.Minimum,
												Content.Maximum
											)
											--
											Content.Current = (Content.State .. Content.Ending)
											Content.Objects["Value"].Text = Content.Current
											--
											Content.Objects["Frame"].Size = UDim2.new(
												(1 - Utils.Math:Percentile(Content.State, Content.Minimum, Content.Maximum)),
												0,
												1,
												0
											)
											--
											Utils:Call(Content.Callback, Content.State)
										end
										--
										function Content:Get(Raw)
											return Content.State
										end
										--
										function Content:Refresh()
											local Location = Utils:Location()
											--
											Content:Set(
												Utils.Math:Interpolate(
													Location.X,
													Content.Objects["Frame"].AbsolutePosition.X,
													(Content.Objects["Outline"].AbsoluteSize.X - 2),
													Content.Minimum,
													Content.Maximum
												)
											)
										end
									end
									--
									do -- Connections
										Utils:Connect(Content.Objects["Button"].MouseEnter, function()
											Menu.Objects:Update(Content.Objects["Outline"], { BorderColor3 = "Accent" })
										end)
										--
										Utils:Connect(Content.Objects["Button"].MouseLeave, function()
											Menu.Objects:Update(Content.Objects["Outline"], { BorderColor3 = "Outline" })
										end)
										--
										Utils:Connect(Content.Objects["Value"].FocusLost, function()
											local Number = tonumber(Content.Objects["Value"].Text)
											--
											if Number then
												Content:Set(Number)
											else
												Content.Objects["Value"].Text = Content.Current
											end
										end)
										--
										Utils:Connect(Content.Objects["Button"].MouseButton1Down, function()
											if (tick() - Content.Tick) <= 0.2 then
												Content.Tick = 0
												--
												if Content.Last then
													Content:Set(Content.Last)
												end
												--
												Content.Objects["Value"]:CaptureFocus()
											else
												Content.Tick = tick()
												--
												Content.Holding = true
												Content.Window.Holds[Content] = "Holding"
												--
												Content:Refresh()
											end
										end)
									end
									--
									do -- Setup
										Content.Section.Content[Content] = true
										--
										if Content.Flag then
											Flags[Content.Flag] = Content
										end
										--
										return Content
									end
								end
								--
								function Menu.Sections:Dropdown(Properties)
									local Properties = (Properties or {})
									local Content = {
										Objects = {},
										--
										Window = self.Window,
										Page = self.Page,
										Section = (self.Section or self),
										--
										State = (Utils:Property(Properties, "State", "Current", "Default", "Def") or 1),
										Shown = (Utils:Property(Properties, "Shown", "Listed", "Viewed") or 5),
										Maximum = (Utils:Property(Properties, "Maximum", "Max", "Most", "Maximal") or 1),
										Minimum = (Utils:Property(Properties, "Minimum", "Min", "Least", "Minimal") or 1),
										Options = (
											Utils:Property(Properties, "Options", "List") or { "1", "2", "3", "4", "5" }
										),
										Selected = (Utils:Property(Properties, "Global", "Selected", "Select")),
										Wide = (Utils:Property(Properties, "Wide", "Wider") or false),
										Flag = (Utils:Property(Properties, "Flag", "Pointer")),
										Callback = (Utils:Property(Properties, "Callback", "Function", "Func", "Return")),
										NoCallback = (Utils:Property(
											Properties,
											"NoCallback",
											"DontCallback",
											"IgnoreCallback",
											"Ignore"
										) or false),
									}
									--
									do -- Objects
										Content.Objects["Holder"] =
											Menu.Objects:Holder(nil, self.Objects["Content"], nil, UDim2.new(1, 0, 0, 25))
										--
										Content.Objects["Outline"], Content.Objects["Frame"] = Menu.Objects:Outline(
											nil,
											Content.Objects["Holder"],
											UDim2.new(0, (Content.Wide and 24 or 36), 0, 4),
											UDim2.new(1, (Content.Wide and -49 or -69), 0, 20),
											"Lighter Background"
										)
										--
										Menu.Objects:Gradient(Color3.fromRGB(0, 0, 0), -90, 0.6, Content.Objects["Frame"])
										--
										Content.Objects["Arrow"] = Menu.Objects:Create("ImageLabel", {
											BackgroundTransparency = 1,
											BorderSizePixel = 0,
											Parent = Content.Objects["Frame"],
											Position = UDim2.new(1, -13, 0, 7),
											Size = UDim2.new(0, 5, 0, 3),
											Image = "rbxassetid://14555080158",
											ImageColor3 = "Accent",
										})
										Content.Objects["Value"] = Menu.Objects:Text(
											false,
											nil,
											Content.Objects["Frame"],
											UDim2.new(0, 6, 0, 0),
											UDim2.new(1, -22, 1, 0),
											"",
											"Light Text",
											nil,
											nil,
											"AtEnd"
										)
										--
										Content.Objects["Button"] = Menu.Objects:Button(Content.Objects["Holder"])
									end
									--
									do -- Functions
										function Content:Set(State, Loaded)
											if
												Loaded
												and typeof(State) == "table"
												and (#State == 0 or table.find(State, "") ~= nil)
											then
												return
											end
											--
											local ProcessedState
											--
											if typeof(State) == "table" then
												ProcessedState = {}
												for i, value in pairs(State) do
													if typeof(value) == "number" then
														ProcessedState[i] = Content.Options[value] or tostring(value)
													else
														ProcessedState[i] = tostring(value)
													end
												end
											else
												if typeof(State) == "number" then
													ProcessedState = Content.Options[State] or tostring(State)
												else
													ProcessedState = tostring(State)
												end
											end
											--
											Content.State = ProcessedState
											--
											local DisplayText = (typeof(ProcessedState) == "table")
													and table.concat(ProcessedState, ", "):gsub("^,%s*", "")
												or ProcessedState
											if not DisplayText or DisplayText == "" then
												DisplayText = "None"
											end
											--
											Content.Objects["Value"].Text = DisplayText
											Utils:Call(Content.Callback, Content.State)
										end
										--
										function Content:Get(Raw)
											return Content.State
										end
									end
									--
									do -- Connections
										Utils:Connect(Content.Objects["Button"].MouseEnter, function()
											Menu.Objects:Update(Content.Objects["Value"], { TextColor3 = "Accent" })
											--
											Menu.Objects:Update(Content.Objects["Outline"], { BorderColor3 = "Accent" })
										end)
										--
										Utils:Connect(Content.Objects["Button"].MouseLeave, function()
											Menu.Objects:Update(Content.Objects["Value"], { TextColor3 = "Light Text" })
											--
											Menu.Objects:Update(Content.Objects["Outline"], { BorderColor3 = "Outline" })
										end)
										--
										Utils:Connect(Content.Objects["Button"].MouseButton1Down, function()
											Content.Window:Open(Content, "Dropdown")
										end)
									end
									--
									do -- Setup
										Content.Section.Content[Content] = true
										--
										if not Content.Selected then
											Content.Selected = Content.Options
										end
										--
										if Content.Flag then
											Flags[Content.Flag] = Content
										end
										--
										return Content
									end
								end
								--
								function Menu.Sections:SearchDown(Properties)
									local Properties, Content = (Properties or {})
									local Box = self:Box({
										Holder = (Utils:Property(
											Properties,
											"Name",
											"Text",
											"Holder",
											"PlaceHolder",
											"PlaceHolderText"
										) or ""),
										Confirm = true,
										Callback = function(State)
											local Searched = {}
											--
											for Index, Value in next, Content.Options do
												if string.find(string.lower(Value), string.lower(State)) then
													Searched[#Searched + 1] = { Index, Value }
												end
											end
											--
											Content.Selected = (
												Utils.Table:Count(Searched) > 0 and Searched or Content.Options
											)
											--
											Content.Window:Close()
											Content.Window:Open(Content, "Dropdown")
										end,
										NoCallback = true,
									})
									--
									Content = self:Dropdown(Properties)
									--
									do -- Objects
										Content.Objects["Extra"] = Box.Objects["Holder"]
									end
									--
									do -- Setup
										return Content
									end
								end
								--
								function Menu.Sections:SearchList(Properties)
									local Properties, Content = (Properties or {})
									local Box = self:Box({
										Holder = (Utils:Property(
											Properties,
											"Name",
											"Text",
											"Holder",
											"PlaceHolder",
											"PlaceHolderText"
										) or ""),
										Wide = (Utils:Property(Properties, "Wide", "Wider") or false),
										Confirm = true,
										Callback = function(State)
											local Searched = {}
											--
											for Index, Value in next, Content.Options do
												if string.find(string.lower(Index), string.lower(State)) then
													Searched[Index] = true
												end
											end
											--
											Content:Refresh(
												Content.Options,
												(Utils.Table:Count(Searched) > 0 and Searched or Content.Options)
											)
										end,
										NoCallback = true,
									})
									--
									Content = self:List(Properties)
									--
									do -- Setup
										return Content
									end
								end
								--
								function Menu.Sections:Divider(Properties)
									local Properties = (Properties or {})
									local Content = {
										Objects = {},
										--
										Window = self.Window,
										Page = self.Page,
										Section = (self.Section or self),
									}
									--
									do -- Objects
										Content.Objects["Holder"] =
											Menu.Objects:Holder(nil, self.Objects["Content"], nil, UDim2.new(1, 0, 0, 15))
										--
										Content.Objects["Outline"], Content.Objects["Frame"] = Menu.Objects:Outline(
											Vector2.new(0.5, 0.5),
											Content.Objects["Holder"],
											UDim2.new(0.5, 0, 0.5, 0),
											UDim2.new(1, -55, 0, 0),
											"Lighter Background"
										)
										--
										Menu.Objects:Gradient(
											Color3.fromRGB(85, 85, 85),
											-90,
											nil,
											Content.Objects["Frame"]
										)
									end
									--
									do -- Setup
										return Content
									end
								end
								--
								do -- Addons
									function Menu.Sections.Addons:Holder(Content)
										local Holder = Content.Objects["Addon_Holder"]
										--
										if Holder then
											return Holder
										else
											Content.Objects["Addon_Holder"] = Menu.Objects:Holder(
												Vector2.new(1, 0),
												Content.Objects["Holder"],
												UDim2.new(1, -12, 0, Content.Offset),
												UDim2.new(0, 0, 1, 0)
											)
											--
											Menu.Objects:List(
												nil,
												Content.Objects["Addon_Holder"],
												"Horizontal",
												"Right",
												"Top"
											)
											--
											return Content.Objects["Addon_Holder"]
										end
									end
									--
									function Menu.Sections.Addons:Picker(Self, Properties)
										local Properties = (Properties or {})
										local Content = {
											Objects = {},
											--
											Window = Self.Window,
											Page = Self.Page,
											Section = Self.Section,
											Self = Self,
											--
											State = (
												Utils:Property(Properties, "State", "Current", "Default", "Color", "Def")
												or Color3.fromRGB(255, 255, 255)
											),
											Alpha = (Utils:Property(Properties, "Transparency", "Transp", "Alpha")),
											Flag = (Utils:Property(Properties, "Flag", "Pointer")),
											Callback = (
												Utils:Property(Properties, "Callback", "Function", "Func", "Return")
											),
										}
										--
										do -- Objects
											Content.Objects["Addon"] = Menu.Sections.Addons:Holder(Self)
											Content.Objects["Holder"] = Menu.Objects:Holder(
												nil,
												Content.Objects["Addon"],
												nil,
												UDim2.new(0, 20, 1, 0)
											)
											--
											Content.Objects["Outline"] = Menu.Objects:Outline(
												Vector2.new(0, 0.5),
												Content.Objects["Holder"],
												UDim2.new(0, 1, 0.5, 0),
												UDim2.new(0, 20, 0, 12),
												nil
											)
											--
											if Content.Alpha then
												Content.Objects["Alpha"] = Menu.Objects:Create("ImageLabel", {
													BackgroundTransparency = 1,
													BorderSizePixel = 0,
													Parent = Content.Objects["Outline"],
													Position = UDim2.new(0, 0, 0, 0),
													Size = UDim2.new(1, 0, 1, 0),
													Image = "rbxassetid://14641036512",
												})
											end
											--
											Content.Objects["Frame"] = Menu.Objects:Frame(
												nil,
												Color3.fromRGB(255, 255, 255),
												nil,
												nil,
												nil,
												0,
												Content.Objects["Outline"],
												UDim2.new(0, 0, 0, 0),
												UDim2.new(1, 0, 1, 0)
											)
											--
											Menu.Objects:Gradient(
												Color3.fromRGB(85, 85, 85),
												-90,
												nil,
												Content.Objects["Frame"]
											)
											--
											Content.Objects["Button"] = Menu.Objects:Button(Content.Objects["Holder"])
										end
										--
										do -- Functions
											function Content:Set(State, Alpha)
												if typeof(State) == "table" then
													Content.State = State
												else
													local Hue, Saturation, Value = State:ToHSV()
													--
													Content.State =
														{ Hue, Saturation, Value, (Content.Alpha and Alpha or nil) }
												end
												--
												local Color =
													Color3.fromHSV(Content.State[1], Content.State[2], Content.State[3])
												--
												Content.Objects["Frame"].BackgroundColor3 = Color
												Content.Objects["Frame"].BackgroundTransparency = (
													Content.Alpha and Content.State[4] or 0
												)
												--
												Utils:Call(Content.Callback, Color, Content.State[4])
											end
											--
											function Content:Get(Raw)
												if Raw then
													return Content.State
												else
													return Color3.fromHSV(
														Content.State[1],
														Content.State[2],
														Content.State[3]
													),
														Content.State[4]
												end
											end
										end
										--
										do -- Connections
											Utils:Connect(Content.Objects["Button"].MouseEnter, function()
												Menu.Objects:Update(Content.Objects["Outline"], { BorderColor3 = "Accent" })
											end)
											--
											Utils:Connect(Content.Objects["Button"].MouseLeave, function()
												Menu.Objects:Update(
													Content.Objects["Outline"],
													{ BorderColor3 = "Outline" }
												)
											end)
											--
											Utils:Connect(Content.Objects["Button"].MouseButton1Down, function()
												Content.Window:Open(Content, "Picker")
											end)
											--
											Utils:Connect(Content.Objects["Button"].MouseButton2Down, function()
												Content.Window:Open(Content, "CopyAndPaste")
											end)
										end
										--
										do -- Setup
											Content.Section.Content[Content] = true
											--
											if Content.Flag then
												Flags[(Self.Flag and (Self.Flag .. " ") or "") .. Content.Flag] = Content
											end
											--
											return Content
										end
									end
									--
									function Menu.Sections.Addons:Bind(Self, Properties)
										local Properties = (Properties or {})
										local Content = {
											Objects = {},
											--
											Active = false,
											--
											Window = Self.Window,
											Page = Self.Page,
											Section = Self.Section,
											Self = Self,
											--
											Name = (Utils:Property(Properties, "Name", "Text")),
											State = (
												Utils:Property(Properties, "State", "Current", "Default", "Bind", "Def")
											),
											Mode = (Utils:Property(Properties, "Mode", "Type") or "On Hold"),
											Disable = (
												Utils:Property(Properties, "Disabled", "Disable", "NoMode") or false
											),
											Flag = (Utils:Property(Properties, "Flag", "Pointer")),
											Callback = (
												Utils:Property(Properties, "Callback", "Function", "Func", "Return")
											),
										}
										--
										do -- Objects
											Content.Objects["Addon"] = Menu.Sections.Addons:Holder(Self)
											Content.Objects["Holder"] = Menu.Objects:Holder(
												nil,
												Content.Objects["Addon"],
												nil,
												UDim2.new(0, 20, 1, 0)
											)
											--
											Content.Objects["Value"] = Menu.Objects:Text(
												false,
												Vector2.new(0, 0.5),
												Content.Objects["Holder"],
												UDim2.new(0, 1, 0.5, 0),
												UDim2.new(0, 18, 0, 9),
												"",
												"Dark Text",
												nil,
												nil,
												nil,
												8
											)
											--
											Content.Objects["Button"] = Menu.Objects:Button(Content.Objects["Holder"])
										end
										--
										do -- Functions
											function Content:Set(State)
												if typeof(State) == "table" then
													Content.Mode = (State[3] or Content.Mode)
													--
													if State[1] == nil and State[2] == nil and State[3] ~= nil then
														return Content:Adjust(Content.Mode)
													end
													--
													Content.State = State
												else
													Content.State = (
														(State and (State.EnumType or State.KeyCode))
															and {
																State.EnumType == Enum.KeyCode and "KeyCode"
																	or "UserInputType",
																State.Name,
															}
														or {}
													)
												end
												--
												Content:Adjust(Content.Mode)
												--
												Content.Short = (
													#Content.State > 0
													and (Menu.Inputs.Shortened[Content.State[2]] or Content.State[2])
												)
												--
												Content.Objects["Value"].Text =
													string.format("[%s]", Content.Short or "...")
												--
												if Content.Item then
													Content.Item:Update()
												end
											end
											--
											function Content:Get(Raw)
												if Raw then
													if #Content.State > 0 then
														return Content.State[1][Content.State[2]]
													end
													return Content.State
												else
													return Content.State
												end
											end
											--
											function Content:Adjust(Mode)
												Content.Mode = Mode
												--
												Content.Active = (
													Content.Mode == "Off Hold"
													or Content.Mode == "Off Toggle"
													or Content.Mode == "Always"
												)
												--
												if Content.Item then
													Content.Item:Update()
												end
											end
											--
											function Content:Update(Type)
												local Previous = Content.Active
												--
												if Content.Mode ~= "Always" then
													if Content.Mode == "On Hold" or Content.Mode == "Off Hold" then
														Content.Active = not Content.Active
													elseif Content.Mode == "On Toggle" or Content.Mode == "Off Toggle" then
														if Type then
															Content.Active = not Content.Active
														end
													end
												end
												--
												if Content.Active ~= Previous then
													Utils:Call(Content.Callback, Content.Active, Previous)
													--
													if Content.Item then
														Content.Item:Update()
													end
												end
											end
											--
											function Content:Activate(State)
												if Content.Mode ~= "Always" then
													Content.State = State
												end
											end
										end
										--
										do -- Connections
											Utils:Connect(Content.Objects["Button"].MouseEnter, function()
												Menu.Objects:Update(Content.Objects["Value"], { TextColor3 = "Accent" })
											end)
											--
											Utils:Connect(Content.Objects["Button"].MouseLeave, function()
												Menu.Objects:Update(Content.Objects["Value"], { TextColor3 = "Dark Text" })
											end)
											--
											Utils:Connect(Content.Objects["Button"].MouseButton1Down, function()
												Content.Window:Open(Content, "Bind")
											end)
											--
											Utils:Connect(Content.Objects["Button"].MouseButton2Down, function()
												if not Content.Disable then
													Content.Window:Open(Content, "BindMode")
												end
											end)
										end
										--
										do -- Setup
											Content.Section.Content[Content] = true
											Content.Window.Binds[Content] = true
											--
											if Content.Flag then
												Flags[(Self.Flag and (Self.Flag .. " ") or "") .. Content.Flag] = Content
											end
											--
											if Flags["Binds"] then
												if not Content.Name then
													Content.Name = (Self.Flag or Content.Flag)
												end
												--
												Content.Item = Flags["Binds"]:Insert(Content)
											end
											--
											return Content
										end
									end
								end
							end
						end
						--
						do -- Presents
							function Menu.Presets:Update(Theme, Color)
								if Menu.Themes[Theme] ~= Color then
									Menu.Themes[Theme] = Color

									for Index, Value in next, Menu.Storage do
										if not Index.Parent then
											Menu.Storage[Index] = nil
										end

										for Index2, Value2 in next, Value do
											if Value2 == Theme then
												Index[Index2] = Color
											end
										end
									end
								end
							end
							--
							function Menu.Presets:Set(Window, Type)
								local Self = self
								local Theme = Self.Themes[Type]
								--
								if Theme then
									for Index, Value in next, Theme do
										Self:Update(Index, Value)
										--
										if Flags[Index] then
											Flags[Index]:Set(Value)
										end
									end
								end
							end
						end
					end)()
				end
			end

			return Utils, Menu, Flags, Character, Visuals2, Env, Window
		end)()
	end
	--
	do
		local _ = function(...)
			return cloneref(game:GetService(...))
		end
		SoldierClassType = {}
		local Players2 = _("Players")
		local Lps = Players2.LocalPlayer
		local Lch = Lps.Character
		local RootPart = Lch.HumanoidRootPart
		local PlayerGui = Lps.PlayerGui
		local MainGui = PlayerGui and PlayerGui:WaitForChild("MainGui")
		local Workspace = _("Workspace")
		local Camera = Workspace.CurrentCamera
		local RunService = _("RunService")
		local UserInputService = _("UserInputService")
		local ReplicatedStorage = _("ReplicatedStorage")
		local Debris = _("Debris")
		local Stats = _("Stats")

		Client = Players2.LocalPlayer

		local MathHuge = math.huge
		local MathRandom = math.random
		local MathFloor = math.floor
		local MathSqrt = math.sqrt
		local Vector2New = Vector2.new
		local Vector3New = Vector3.new
		local CFrameNew = CFrame.new
		local IPairs = ipairs
		local ToString = tostring
		local ToNumber = tonumber
		local Unpack = unpack
		local StringFormat = string.format
		local GetGc = getgc

		local vfxNames = {}

		local OriginalHealthStorage = {}

		local FovOutline, OutlineRounding, Outline, FovInline, InlineRounding, Inline, Gradient, OldCreateBullet

		Cheat = {
			Connections = {},
			IsLocked = false,
			LockedTarget = nil,
			Options = {
				SilentAim = {
					Enabled = true,
					EnabledKB = true,
					HitPart = "Head",
					TargetAI = true,
					VisCheck = false,
					max_dist = 1,
					Fov = {
						Enabled = true,
						Size = 100,
						Type = "Normal",
						Use = false,
						Visible = true,
						Outline = true,
						Gradient_rotate = true,
						Rotation_speed = 50,
						Gradient_clr1 = Color3.fromRGB(255, 255, 255),
						Gradient_clr2 = Color3.fromRGB(0, 0, 0),
						Outline_clr = Color3.fromRGB(0, 0, 0),
						Position = "Middle",
						InTrans = 0,
						OutTrans = 0,
					},
					Prediction = {
						Enabled = false,
					},
					HitChance = {
						Amount = 100,
						Mode = "Activation",
						ClosestPart = false,
						List = {
							"Head",
							"HumanoidRootPart",
							"UpperTorso",
							"LowerTorso",
							"LeftUpperArm",
							"LeftLowerArm",
							"LeftHand",
							"RightUpperArm",
							"RightLowerArm",
							"RightHand",
							"LeftUpperLeg",
							"LeftLowerLeg",
							"LeftFoot",
							"RightUpperLeg",
							"RightLowerLeg",
							"RightFoot",
							"Closest",
						},
					},
				},
				Aimbot = {
					Enabled = false,
					EnabledKB = "None",
					IsLocked = false,
					LockedTarget = nil,
					Connections = {},
					ClosestPart = false,
					HitPart = "Head",
					Vis_Check = false,
					max_dist = 1,
					List = {
						"Head",
						"HumanoidRootPart",
						"UpperTorso",
						"LowerTorso",
						"LeftUpperArm",
						"LeftLowerArm",
						"LeftHand",
						"RightUpperArm",
						"RightLowerArm",
						"RightHand",
						"LeftUpperLeg",
						"LeftLowerLeg",
						"LeftFoot",
						"RightUpperLeg",
						"RightLowerLeg",
						"RightFoot",
					},
					Prediction = {
						Enabled = false,
					},
					Smoothing = {
						Enabled = false,
						Amount = 50,
					},
				},
				GunMods = {
					InstantLean = { Value = false },
					NoBobbing = { Value = false },
					InstantAim = { Value = false },
					NoRecoil = { Value = false },
					RecoilVal = 0,
					InstantEquip = { Value = false },
					UnlockFiremodes = { Value = false },
					Rapidfire = { Value = false },
					NoSway = { Value = false },
					RemoveObstructions = { Value = false },
					InstantReload = { Value = false },
					AlwaysHeadshot = false,
					InstaBullet = false,
					doubleTap = false,
					NoSprintOffsett = false,
				},
				Misc = {
					nodrown = false,
					forcepitch = false,
					Weapon = {
						-- hits
						Hitmarkers = true,
						Logs = true,
						HitmarkerColor = Color3.fromRGB(255, 0, 0),
						DamageNumber = true,
						DamageColor = Color3.fromRGB(255, 0, 0),
						Time = 1.5,
						MarkerTime = 1.5,
						-- effects
						EffectEnabled = true,
						Effect = "KO",
						EffectTime = 1.5,
						EffectColor = Color3.fromRGB(0, 255, 0),
						-- chams
						ChamsEnabled = false,
						ChamsColor = Color3.fromRGB(255, 255, 255),
						ChamsLife = 3,
					},
					Custom = {
						-- animated textures
						ShaderEnabled = false,
						ShaderTile = 0,
						ShaderSpeed = 0,
						Shader = "Galaxy",
						-- weapon sound customization
						customShootSound = false,
						shootsoundVol = 1, -- 0, 1
						shootsoundSpeed = 1, -- 0, 5
						shootSoundList = {
							["AWP"] = "2753888131",
							["AK47-CS:GO"] = "2476570846",
							["Deagle"] = "82286818216627",
							["USP-S"] = "4108910200",
							["HK416"] = "8241511326",
							["M4A1"] = "9057685835",
							["AK47-Rust"] = "7011577038",
						}, -- also allow custom shoot sounds from a textbox
						selectedShootSound = "",
						-- weapon stickers like in cs
						stickersEnabled = false,
						stickerList = {}, -- jusst add some decals xd, also custom from textbox
						selectedStickers = {}, -- max 4
						--  weapon charms like in cs
						weaponCharmsEnabled = false,
						charmsList = {}, -- gotta make them in studio.. and physics...
						selectedcharm = "", -- only one
						-- waepon effects like skins in fallen
						effectsEnabled = false,
						effectsList = { "Glow", "Aura", "Flame", "Black Ice" },
						selectedEffect = "",
					},
					AspectRatio = {
						Enabled = false,
						Horizontal = 0,
						Vertical = 0,
					},
				},
			},
			rage = {
				AntiAim = {
					Visualize = false,
					Pitchenabled = false,
					pitchBase = "Zero", -- Up,Down,Zero,Custom,Random,Jitter
					customVal = 0,
					Jitter1 = 0,
					Jitter2 = 1,
					SpoofVelocity = false,
					SpoofVelocityKB = false,
					VelocityFactor = 0,
					VelocityTimeout = false,
					VelocityTimeoutTime = 0,
					Yaw = {
						Enabled = false,
						Value = 0,
						Mode = "Static", --Static,Jitter,Random,
						LerpMin = 0,
						LerpMax = 0,
					},
					Roll = {
						Enabled = false,
						Value = 0,
						Mode = "Static", -- Static,Jitter,Random
						LerpMin = 0,
						LerpMax = 0,
					},
				},
			},
			storage = {
				lastHitPart = nil,
				lastVelocity = 0,
				LastYawValue = false,
			},
		}

		--// Damage numbers, Hit Effects, Hitmarker
		do
			LPH_NO_VIRTUALIZE(function()
				local GameFramework = { Last = tick() }
				local Visuals = {}
				local EffectHolder = game:GetObjects("rbxassetid://17192721766")[1]
				Visuals.ESPContainer = Instance.new("ScreenGui")
				do
					Visuals.ESPContainer.Parent = gethui()
					Visuals.ESPContainer.Name = "9391203j312i3kashd9q312093ajsdokake219183213h"
					Visuals.ESPContainer.IgnoreGuiInset = true
				end
				do --// Tables
					Visuals.Hitmarkers = {}
					Visuals.HitDamages = {}
					Visuals.ColorSets = {
						{ Color3.fromRGB(200, 80, 0), Color3.fromRGB(120, 119, 255) },
						{ Color3.fromRGB(50, 69, 205), Color3.fromRGB(120, 119, 255) },
						{ Color3.fromRGB(100, 101, 255), Color3.fromRGB(255, 60, 1) },
					}
				end

				function MakeEffect(c)
					local Effects = EffectHolder.Clone(EffectHolder)
					local EffectName = Cheat.Options.Misc.Weapon.Effect
					Effects.CFrame = c
					Effects.Parent = workspace
					Effects[EffectName].Parent = Effects.Attachment
					for i, v in ipairs(Effects.Attachment[EffectName].GetChildren(Effects.Attachment[EffectName])) do
						v.Color = ColorSequence.new(Cheat.Options.Misc.Weapon.EffectColor)
						v.Emit(v, 1)
					end
					Effects.Attachment[EffectName].Parent = Effects

					task.delay(Cheat.Options.Misc.Weapon.EffectTime, function()
						Effects.Destroy(Effects)
					end)
				end

				function NewHitmarker(Position)
					Visuals.Hitmarkers[#Visuals.Hitmarkers + 1] = {
						Position = Position,
						StartedAt = tick(),
						IsFading = false,
						FadeTick = 0,
						Hitmarker = Hitmarker,
					}
				end

				function GetBelt(Part)
					if Part.Name:find("Head") or Part.Name:find("Face") then
						for Index, Value in next, Part.Parent:GetChildren() do
							if Value:IsA("Model") and Value:GetAttribute("ItemType") then
								if Value:GetAttribute("ItemType") == "Headware" then
									return Value.Name
								end
							end
						end
					elseif Part.Name:find("Torso") then
						for Index, Value in next, Part.Parent:GetChildren() do
							if Value:IsA("Model") and Value:GetAttribute("ItemType") then
								if Value:GetAttribute("ItemType") == "ChestRig" then
									return Value.Name
								end
							end
						end
					elseif Part.Name:find("Leg") then
						for Index, Value in next, Part.Parent:GetChildren() do
							if Value:IsA("Model") and Value:GetAttribute("ItemType") then
								if Value:GetAttribute("ItemType") == "LegBelt" then
									return Value.Name
								end
							end
						end
					end

					return nil
				end

				local rp = game:GetService("ReplicatedStorage")

				local realAmmoTypes = rp:FindFirstChild("realAmmoTypes")
					or (rp:FindFirstChild("AmmoTypes") and rp:FindFirstChild("AmmoTypes"):Clone())
				if realAmmoTypes then
					realAmmoTypes.Name = "realAmmoTypes"
					realAmmoTypes.Parent = rp
				end

				local RunService = game:GetService("RunService")

				local function gettool()
					local player = game:GetService("Players").LocalPlayer
					local playerData = game:GetService("ReplicatedStorage").Players:FindFirstChild(player.Name)

					if playerData and playerData:FindFirstChild("Status") then
						local gameplayVars = playerData.Status:FindFirstChild("GameplayVariables")
						if gameplayVars then
							local equippedTool = gameplayVars:FindFirstChild("EquippedTool")
							if equippedTool and equippedTool.Value ~= nil and equippedTool.Value ~= "" then
								return tostring(equippedTool.Value)
							end
						end
					end

					return "None"
				end

				local previousItem = "None"

				local renderSteppedConnection
				renderSteppedConnection = RunService.RenderStepped:Connect(function()
					local currentItem = gettool()

					if currentItem ~= previousItem then
						previousItem = currentItem
						ItemMain = currentItem
					end
				end)

				--local function stopToolMonitoring()
				--	if renderSteppedConnection then
				--		renderSteppedConnection:Disconnect()
				--		renderSteppedConnection = nil
				--	end
				--end

				ItemMain = gettool()

				local function UpdateWeaponStats()
					if ItemMain == "AKMN" or ItemMain == "AKM" then
						AmmoCool = "762x39T`racer"
						DamageCool = 57
						PenetrationCool = 28
					elseif ItemMain == "ADAR15" or ItemMain == "M4" then
						AmmoCool = "556x45Tracer"
						DamageCool = 60
						PenetrationCool = 20
					elseif ItemMain == "PKM" or ItemMain == "Mosin" or ItemMain == "SVD" then
						AmmoCool = "762x54Tracer"
						DamageCool = 71
						PenetrationCool = 30
					elseif ItemMain == "R700" or ItemMain == "TFZ98S" then
						AmmoCool = "338T"
						DamageCool = 121
						PenetrationCool = 31
					elseif ItemMain == "FAL" then
						AmmoCool = "762x51Tracer"
						DamageCool = 65
						PenetrationCool = 29
					elseif ItemMain == "AsVal" then
						AmmoCool = "9x39Z"
						DamageCool = 55
						PenetrationCool = 39
					elseif ItemMain == "Saiga12" or ItemMain == "IZh12" or ItemMain == "IZh81" then
						AmmoCool = "12gaBuckshot"
						DamageCool = 34
						PenetrationCool = 16
					elseif ItemMain == "MP5SD" then
						AmmoCool = "9x19Tracer"
						DamageCool = 55
						PenetrationCool = 10
					elseif ItemMain == "PPSH41" or "TT33" then
						AmmoCool = "762x25Tracer"
						DamageCool = 55
						PenetrationCool = 11
					elseif ItemMain == "RPG7" then
						AmmoCool = "PG7"
						DamageCool = 420
						PenetrationCool = 100
					elseif ItemMain == "TFZ0" or ItemMain == "VZ61" then
						AmmoCool = "9x18Tracer"
						DamageCool = 48
						PenetrationCool = 9
					elseif ItemMain == "Makarov" then
						AmmoCool = "9x19Tracer"
						DamageCool = 34
						PenetrationCool = 9
					elseif ItemMain == "VZ61" then
						AmmoCool = "9x19Tracer"
						DamageCool = 48
						PenetrationCool = 9
					elseif ItemMain == "MK23" then
						AmmoCool = "45Tracer"
						DamageCool = 63
						PenetrationCool = 13
					elseif ItemMain == "MP443" then
						AmmoCool = "9x19Tracer"
						DamageCool = 55
						PenetrationCool = 10
					else
						AmmoCool = "762x39Tracer"
						DamageCool = 0
						PenetrationCool = 0
					end
				end

				renderSteppedConnection = RunService.RenderStepped:Connect(function()
					local currentItem = gettool()

					if currentItem ~= previousItem then
						previousItem = currentItem
						ItemMain = currentItem

						UpdateWeaponStats()
					end
				end)

				function GetBulletStats(Ammo)
					if not realAmmoTypes then
						return { Damage = DamageCool, Penetration = PenetrationCool }
					end

					local Info = realAmmoTypes:FindFirstChild(tostring(Ammo))
					if Info then
						return {
							Damage = Info:GetAttribute("Damage"),
							Penetration = Info:GetAttribute("BeltPen"),
						}
					end
					return { Damage = DamageCool, Penetration = PenetrationCool }
				end

				function CalculateDamage(BulletDamage, BeltPentration, BeltProtection, IsHeadshot)
					local Multiplier = IsHeadshot and 2 or 1

					local BeltReduction = (BeltProtection - BeltPentration) / 100

					local EffectiveDamage = BulletDamage * (1 - BeltReduction)

					local FinalDamage = EffectiveDamage * Multiplier

					return FinalDamage
				end

				function NewHitDamage(Position, Amount, Part)
					local Data = {
						FadeTick = 0,
						IsFading = false,
						StartedAt = tick(),
						Label = nil,
						RandomizedSpot = Vector2.new(10, 0) + Vector2.new(math.random(1, 3), math.random(1, 3)),
						Position = Position,
						Amount = Amount,
						Part = Part,
					}

					Visuals.HitDamages[#Visuals.HitDamages + 1] = Data
				end

				function UpdateDamage()
					for Index, Data in next, Visuals.HitDamages do
						if not Data.Label then
							local Label = Instance.new("TextLabel", Visuals.ESPContainer)
							Label.BackgroundTransparency = 1
							Label.FontFace = Visuals2.Fonts["SmallestPixel"][1]
							Label.TextStrokeTransparency = 0
							Label.TextColor3 = Cheat.Options.Misc.Weapon.DamageColor
							Label.TextSize = 9
							Label.Text = tostring(math.floor(Data.Amount))
							Data.Label = Label
						end

						local Label = Data.Label
						local Transparency = 0
						local RandomizedSpot = Data.RandomizedSpot
						local Connection

						do --// Setting properties
							local ScreenPosition, IsOnScreen = Camera:WorldToViewportPoint(Data.Position)

							Label.Visible = IsOnScreen

							if Label.Visible then
								local OffsetY = (tick() - Data.StartedAt) * 25
								Label.Position = UDim2.fromOffset(
									ScreenPosition.X + RandomizedSpot.X,
									ScreenPosition.Y - OffsetY + RandomizedSpot.Y
								)
							end

							if tick() - Data.StartedAt > Cheat.Options.Misc.Weapon.MarkerTime and not Data.IsFading then
								Data.FadeTick = tick()
								Data.IsFading = true
							end

							if Data.IsFading then
								local TimeDifference = tick() - Data.FadeTick
								Transparency = math.min(1, TimeDifference / Cheat.Options.Misc.Weapon.Time)
							end

							Label.TextTransparency = Transparency

							if Label.TextTransparency == 1 then
								Label:Destroy()
								Visuals.HitDamages[Index] = nil
							end
						end
					end
				end

				function UpdateHitmarkers()
					for Index, Data in pairs(Visuals.Hitmarkers) do
						task.spawn(function()
							if not Data.Hitmarker then
								local Hitmarker = Instance.new("ImageLabel", Visuals.ESPContainer)
								Hitmarker.Image = "http://www.roblox.com/asset/?id=13298929624"
								Hitmarker.BackgroundTransparency = 1
								Hitmarker.Size = UDim2.new(0, 50, 0, 50)
								Hitmarker.Visible = false
								Hitmarker.Rotation = 45
								Hitmarker.AnchorPoint = Vector2.new(0.5, 0.5)

								Data.Hitmarker = Hitmarker
							end
							local Hitmarker = Data.Hitmarker
							local Position, IsOnScreen = Camera:WorldToViewportPoint(Data.Position)
							Hitmarker.Visible = IsOnScreen
							local Transparency = 0

							if tick() - Data.StartedAt > Cheat.Options.Misc.Weapon.MarkerTime and not Data.IsFading then
								Data.FadeTick = tick()
								Data.IsFading = true
							end

							if Data.IsFading then
								local TimeDifference = tick() - Data.FadeTick
								Transparency = math.min(1, TimeDifference / Cheat.Options.Misc.Weapon.Time)
							end

							if Hitmarker.Visible then
								Hitmarker.ImageTransparency = Transparency
								Hitmarker.Position = UDim2.new(0, Position.X, 0, Position.Y)
								Hitmarker.ImageColor3 = Cheat.Options.Misc.Weapon.HitmarkerColor
							end

							if Hitmarker.ImageTransparency == 1 then
								Visuals.Hitmarkers[Index] = nil
								Hitmarker:Destroy()
							end
						end)
					end
				end

				local Players = game:GetService("Players")
				local Client = Players.LocalPlayer

				local Namecall
				Namecall = hookmetamethod(game, "__namecall", function(self, ...)
					local Args = { ... }
					local Method = getnamecallmethod()
					local ExecutorCall = checkcaller()
					if not ExecutorCall then
						if Method == "FireServer" then
							if self.Name == "ProjectileInflict" then
								if Args[1] == Client.Character.HumanoidRootPart then
									return coroutine.yield()
								end

								do -- hit effects
									task.spawn(function()
										if Cheat.Options.Misc.Weapon.EffectEnabled then
											MakeEffect(Args[1].CFrame)
										end
									end)
								end

								--if Cheat.Options.Misc.Weapon.Logs then
								--	--print("hit", Args[1], Args[1].Position)
								--	Menu:Toast({Text = 'Hit', Duration = 1})
								--end

								if Cheat.Options.Misc.Weapon.Hitmarkers or Cheat.Options.Misc.Weapon.DamageNumber then
									task.spawn(function()
										if Cheat.Options.Misc.Weapon.Hitmarkers then
											NewHitmarker(Args[1].Position)
										end

										if Cheat.Options.Misc.Weapon.DamageNumber then
											task.spawn(function()
												local Part = Args[1]
												local IsHeadshot = (Part.Name:find("Head") or Part.Name:find("Face"))
														and true
													or false
												local Belt = GetBelt(Part)
												local BeltProtection = 1
												--if Belt then
												--	BeltProtection = GetBeltProtection(Belt, Part.Parent.Name)
												--end

												local BulletStats = GetBulletStats(GameFramework.LastBulletType)
												local Damage = BulletStats.Damage
												local BeltPen = BulletStats.Penetration

												if BeltProtection ~= 1 then
													local FinalizedDamage =
														CalculateDamage(Damage, BeltPen, BeltProtection, IsHeadshot)

													Damage = FinalizedDamage
												else
													if IsHeadshot then
														Damage = Damage * 2
													end
												end
												NewHitDamage(Part.Position, Damage, Args[1])
											end)
										end
									end)
								end
							end
						end
					end

					return Namecall(self, unpack(Args))
				end)

				game:GetService("RunService").Heartbeat:Connect(function()
					UpdateDamage()
				end)

				game:GetService("RunService").Heartbeat:Connect(function()
					UpdateHitmarkers()
				end)
			end)()
		end

		--for name, _ in pairs(Cheat.Options.GunMods.ShootSoundList) do
		--	table.insert(vfxNames, name)
		--end

		local GameName = "Project Delta"
		local GameFramework = {
			Originals = {
				Springs = {
					leanAlpha = {
						Speed = 4,
						Force = 50,
					},
					jumpTilt = {
						Speed = 4,
						Force = 90,
					},
					walkCycle = {
						Speed = 4,
						Force = 50,
					},
					sprintCycle = {
						Speed = 4,
						Force = 50,
					},
					strafeTilt = {
						Speed = 4,
						Force = 50,
					},
					cameraRecoil = {
						Force = 100,
						Speed = 6,
					},
					recoilPos = {
						Speed = 5,
						Force = 75,
					},
					recoilRot = {
						Speed = 3,
						Force = 35,
					},
					sway = {
						Force = 100,
						Speed = 4,
					},
					wallTouchTilt = {
						Speed = 4,
						Force = 50,
					},
				},
			},
		}

		local GlobalCache = {
			fps = 0,
			fps_interval = 0,
			ping = 0,
			time = "00:00:00",
		}

		do
			local function SetupGameFramework()
				if GameName == "Project Delta" then
					for _, Value in next, GetGc(true) do
						if typeof(Value) == "table" then
							if rawget(Value, "updateClient") then
								GameFramework.FPS = Value
							end
							if rawget(Value, "springs") and rawget(Value.springs, "sway") then
								GameFramework.CurrentData = Value
							end
						end
					end

					if GameFramework.FPS and GameFramework.CurrentData then
						local OldUpdateClient = GameFramework.FPS.updateClient
						GameFramework.FPS.updateClient = function(...)
							local Args = { ... }
							local WeaponData = Args[1]

							if GameFramework.CurrentData then
								if Cheat.Options.GunMods.InstantLean.Value then
									GameFramework.CurrentData.springs.leanAlpha.Force = 25
									GameFramework.CurrentData.springs.leanAlpha.Speed = 50
								else
									GameFramework.CurrentData.springs.leanAlpha.Force =
										GameFramework.Originals.Springs.leanAlpha.Force
									GameFramework.CurrentData.springs.leanAlpha.Speed =
										GameFramework.Originals.Springs.leanAlpha.Speed
								end

								if Cheat.Options.GunMods.NoBobbing.Value then
									GameFramework.CurrentData.springs.jumpTilt.Force = 0
									GameFramework.CurrentData.springs.jumpTilt.Speed = 0
									GameFramework.CurrentData.springs.walkCycle.Speed = 0
									GameFramework.CurrentData.springs.walkCycle.Force = 0
									GameFramework.CurrentData.springs.sprintCycle.Speed = 0
									GameFramework.CurrentData.springs.sprintCycle.Force = 0
									GameFramework.CurrentData.springs.strafeTilt.Speed = 0
									GameFramework.CurrentData.springs.strafeTilt.Force = 0
								else
									GameFramework.CurrentData.springs.jumpTilt.Force =
										GameFramework.Originals.Springs.jumpTilt.Force
									GameFramework.CurrentData.springs.jumpTilt.Speed =
										GameFramework.Originals.Springs.jumpTilt.Speed
									GameFramework.CurrentData.springs.walkCycle.Speed =
										GameFramework.Originals.Springs.walkCycle.Speed
									GameFramework.CurrentData.springs.walkCycle.Force =
										GameFramework.Originals.Springs.walkCycle.Force
									GameFramework.CurrentData.springs.sprintCycle.Speed =
										GameFramework.Originals.Springs.sprintCycle.Speed
									GameFramework.CurrentData.springs.sprintCycle.Force =
										GameFramework.Originals.Springs.sprintCycle.Force
									GameFramework.CurrentData.springs.strafeTilt.Speed =
										GameFramework.Originals.Springs.strafeTilt.Speed
									GameFramework.CurrentData.springs.strafeTilt.Force =
										GameFramework.Originals.Springs.strafeTilt.Force
								end

								if Cheat.Options.GunMods.NoRecoil.Value then
									GameFramework.CurrentData.springs.cameraRecoil.Force = 0
									GameFramework.CurrentData.springs.cameraRecoil.Speed = 0
									GameFramework.CurrentData.springs.recoilRot.Force = 0
									GameFramework.CurrentData.springs.recoilRot.Speed = 0
								else
									GameFramework.CurrentData.springs.cameraRecoil.Force =
										GameFramework.Originals.Springs.cameraRecoil.Force
									GameFramework.CurrentData.springs.cameraRecoil.Speed =
										GameFramework.Originals.Springs.cameraRecoil.Speed
									GameFramework.CurrentData.springs.recoilRot.Force =
										GameFramework.Originals.Springs.recoilRot.Force
									GameFramework.CurrentData.springs.recoilRot.Speed =
										GameFramework.Originals.Springs.recoilRot.Speed
								end

								if Cheat.Options.GunMods.NoSway.Value then
									GameFramework.CurrentData.springs.sway.Position = Vector3.new(0, 0, 0)
									GameFramework.CurrentData.springs.sway.Speed = 0
								else
									GameFramework.CurrentData.springs.sway.Speed =
										GameFramework.Originals.Springs.sway.Speed
								end

								if Cheat.Options.GunMods.RemoveObstructions.Value then
									GameFramework.CurrentData.springs.wallTouchTilt.Force = 0
									GameFramework.CurrentData.springs.wallTouchTilt.Speed = 0
								else
									GameFramework.CurrentData.springs.wallTouchTilt.Force =
										GameFramework.Originals.Springs.wallTouchTilt.Force
									GameFramework.CurrentData.springs.wallTouchTilt.Speed =
										GameFramework.Originals.Springs.wallTouchTilt.Speed
								end
							end

							if WeaponData then
								if Cheat.Options.GunMods.InstantEquip.Value then
									WeaponData.EquipTValue = 100
								end

								if
									Cheat.Options.GunMods.InstantAim.Value
									and rawget(WeaponData, "AimInSpeed")
									and rawget(WeaponData, "AimOutSpeed")
								then
									WeaponData.AimInSpeed = 0
									WeaponData.AimOutSpeed = 0
								end

								if Cheat.Options.GunMods.UnlockFiremodes.Value and rawget(WeaponData, "FireModes") then
									WeaponData.FireModes = { "Auto", "Semi" }
								end

								if Cheat.Options.GunMods.Rapidfire.Value and rawget(WeaponData, "FireModes") then
									WeaponData.FireModes = { "Auto" }
								end

								if
									Cheat.Options.GunMods.RemoveObstructions.Value and rawget(WeaponData, "TouchWallPosY")
								then
									WeaponData.TouchWallPosY = 0
									WeaponData.TouchWallPosZ = 0
									WeaponData.TouchWallRotX = 0
									WeaponData.TouchWallRotY = 0
								end
							end

							return OldUpdateClient(Unpack(Args))
						end
					end
				end
			end

			SetupGameFramework()

			Client.CharacterAdded:Connect(function()
				GameFramework.CurrentData = nil
				task.wait(3)
				repeat
					task.wait()
				until Client.Character and Client.Character:FindFirstChildOfClass("Humanoid")
				SetupGameFramework()
			end)
		end
		local Camera = workspace.CurrentCamera

		local GetBarrel = function()
			local Viewmodel = Camera:FindFirstChild("ViewModel")
			if not Viewmodel then
				return nil
			end

			local Item = Viewmodel:FindFirstChild("Item")
			if not Item then
				return nil
			end

			for _, descendant in ipairs(Item:GetDescendants()) do
				if descendant.Name == "Barrel" then
					return descendant
				end
			end

			local Attachments = Item:FindFirstChild("Attachments")
			local Front = Attachments and Attachments:FindFirstChild("Front")

			if Front then
				for _, child in ipairs(Front:GetDescendants()) do
					if child.Name == "Barrel" or child.Name == "MuzzleOffset" or child.Name == "Front" then
						return child
					end
				end
			end

			return nil
		end

		local function GetBullet(Attribute)
			local AttributeValue = nil
			local PlayerDataFolder = ReplicatedStorage:FindFirstChild("Players2")
			if PlayerDataFolder then
				local Players2tatus = PlayerDataFolder:FindFirstChild(Client.Name)
				if Players2tatus then
					local Status = Players2tatus:FindFirstChild("Status")
					if Status then
						local GameplayVariables = Status:FindFirstChild("GameplayVariables")
						if GameplayVariables then
							local EquippedTool = GameplayVariables:FindFirstChild("EquippedTool")
							if EquippedTool and EquippedTool.Value then
								local Inventory = Players2tatus:FindFirstChild("Inventory")
								if Inventory then
									local CurrentEquipped = Inventory:FindFirstChild(ToString(EquippedTool.Value))
									if CurrentEquipped then
										local Magazine = CurrentEquipped:FindFirstChild("Attachments")
												and CurrentEquipped.Attachments:FindFirstChild("Magazine")
												and CurrentEquipped.Attachments.Magazine:FindFirstChildOfClass(
													"StringValue"
												)
												and CurrentEquipped.Attachments.Magazine
													:FindFirstChildOfClass("StringValue")
													:FindFirstChild("ItemProperties")
												and CurrentEquipped.Attachments.Magazine
													:FindFirstChildOfClass("StringValue").ItemProperties
													:FindFirstChild("LoadedAmmo")
											or CurrentEquipped:FindFirstChild("ItemProperties")
												and CurrentEquipped.ItemProperties:FindFirstChild("LoadedAmmo")
										if Magazine then
											local BulletType = Magazine:FindFirstChild("1")
											if BulletType then
												local AmmoTypesFolder = ReplicatedStorage:FindFirstChild("AmmoTypes")
												if AmmoTypesFolder then
													local AmmoType =
														AmmoTypesFolder:FindFirstChild(BulletType:GetAttribute("AmmoType"))
													if AmmoType then
														AttributeValue = AmmoType:GetAttribute(Attribute)
													end
												end
											end
										end
									end
								end
							end
						end
					end
				end
			end
			return AttributeValue
		end

		local Namecall
		Namecall = hookmetamethod(game, "__namecall", function(self, ...)
			local Args = { ... }
			local Method = getnamecallmethod()
			local ExecutorCall = checkcaller()

			if not ExecutorCall then
				if Method == "GetAttribute" then
					if Args[1] == "UpAngle" and Cheat.Options.Misc.forcepitch then
						return 0
					end
				end

				if Method == "FireServer" and self.Name == "Drowning" and Cheat.Options.Misc.nodrown then
					return
				end
			end

			if Method == "InvokeServer" then
				if self.Name == "FireProjectile" then
					task.spawn(function() end)
				end
			end

			return Namecall(self, unpack(Args))
		end)

		function UpdateRatio()
			if Cheat.Options.Misc.AspectRatio.Enabled then
				local X, Y, Z, R00, R01, R02, R10, R11, R12, R20, R21, R22 = Camera.CFrame:GetComponents()
				Camera.CFrame = CFrameNew(
					X,
					Y,
					Z,
					R00 * (Cheat.Options.Misc.AspectRatio.Horizontal / 100),
					R01 * (Cheat.Options.Misc.AspectRatio.Vertical / 100),
					R02,
					R10,
					R11 * (Cheat.Options.Misc.AspectRatio.Vertical / 100),
					R12,
					R20 * (Cheat.Options.Misc.AspectRatio.Horizontal / 100),
					R21 * (Cheat.Options.Misc.AspectRatio.Vertical / 100),
					R22
				)
			end
		end

		function MathJitter(num1b, numb2)
			local toggle = false
			return function()
				toggle = not toggle
				return toggle and num1b or numb2
			end
		end

		JitterYaw = MathJitter(Cheat.rage.AntiAim.Roll.LerpMin, Cheat.rage.AntiAim.Roll.LerpMax)
		JitterRoll = MathJitter(Cheat.rage.AntiAim.Roll.LerpMin, Cheat.rage.AntiAim.Roll.LerpMax)

		local function DesyncThink()
			local CFX = 0
			local CFY = 0
			local CFZ = 0

			if Cheat.rage.AntiAim.Yaw.Enabled then
				if Cheat.rage.AntiAim.Yaw.Mode == "Static" then
					CFY = Cheat.rage.AntiAim.Yaw.Value
				elseif Cheat.rage.AntiAim.Yaw.Mode == "Jitter" then
					CFY = JitterYaw()
				elseif Cheat.rage.AntiAim.Yaw.Mode == "Random" then
					CFY = math.random(-360, 360)
				end
			else
				CFY = 0
			end

			if Cheat.rage.AntiAim.Roll.Enabled then
				if Cheat.rage.AntiAim.Roll.Mode == "Static" then
					CFZ = Cheat.rage.AntiAim.Roll.Value
				elseif Cheat.rage.AntiAim.Roll.Mode == "Jitter" then
					CFZ = JitterRoll()
				elseif Cheat.rage.AntiAim.Roll.Mode == "Random" then
					CFZ = math.random(-360, 360)
				end
			else
				CFZ = 0
			end

			local DesycVals = CFrameNew(0, 0, 0)

			DesycVals *= CFrame.Angles(0, math.rad(CFY), math.rad(CFZ))

			RootPart.CFrame *= DesycVals
		end

		function getRandomJitter(num1, num2)
			local jitterMin = num1
			local jitterMax = num2

			local minVal, maxVal = math.min(jitterMin, jitterMax), math.max(jitterMin, jitterMax)

			return minVal + (maxVal - minVal) * math.random()
		end

		local function SolveQuadratic(A, B, C)
			local discriminant = B ^ 2 - 4 * A * C
			if discriminant < 0 then
				return nil, nil
			end

			local discRoot = MathSqrt(discriminant)
			local root1 = (-B - discRoot) / (2 * A)
			local root2 = (-B + discRoot) / (2 * A)

			return root1, root2
		end

		local function GetBallisticFlightTime(Direction, Gravity, ProjectileSpeed)
			local root1, root2 = SolveQuadratic(
				Gravity:Dot(Gravity) / 4,
				Gravity:Dot(Direction) - ProjectileSpeed ^ 2,
				Direction:Dot(Direction)
			)

			if root1 and root2 then
				if root1 > 0 and root1 < root2 then
					return math.sqrt(root1)
				elseif root2 > 0 and root2 < root1 then
					return math.sqrt(root2)
				end
			end

			return 0
		end

		function PredictionDrop(Origin, Destination, Speed, Drop)
			local Distance = (Camera.CFrame.Position - Destination).Magnitude
			local TimeToHit = (Distance / Speed)

			local NewSpeed = Speed - 0.013 * Speed ^ 2 * TimeToHit ^ 2
			TimeToHit += (Distance / NewSpeed)

			local DropTiming = Drop * TimeToHit ^ 2

			if tostring(DropTiming):find("nan") then
				return 0
			end
			return DropTiming
		end

		local function Predict(Target, Origin, ProjectileSpeed, Acceleration)
			local Gravity = Vector3New() + Vector3.yAxis * (Acceleration * 2)
			local Time = GetBallisticFlightTime(Target.Position - Origin, Gravity, ProjectileSpeed)

			return Target.Position + (Target.Velocity * Time)
		end

		local function CreateInstance(Class, Properties)
			local InstanceObject = typeof(Class) == "string" and Instance.new(Class) or Class
			for Property, Value in pairs(Properties) do
				InstanceObject[Property] = Value
			end
			return InstanceObject
		end

		local IsTargetVisible = function(Target)
			local TargetCharacter = Target
			if Target and Target.IsA(Target, "Player") then
				TargetCharacter = Target.Character
			end
			if not TargetCharacter or not TargetCharacter.IsA(TargetCharacter, "Model") then
				return false
			end
			local CameraPosition = Camera.CFrame.Position
			local RaycastParamsInstance = RaycastParams.new()
			RaycastParamsInstance.FilterType = Enum.RaycastFilterType.Exclude
			RaycastParamsInstance.FilterDescendantsInstances = { Players2.LocalPlayer.Character }
			for _, PartName in ipairs(Cheat.Options.SilentAim.HitChance.List) do
				local TargetPart = TargetCharacter.FindFirstChild(TargetCharacter, PartName)
				if TargetPart and TargetPart.IsA(TargetPart, "BasePart") then
					local Direction = TargetPart.Position - CameraPosition
					local Result = Workspace.Raycast(Workspace, CameraPosition, Direction, RaycastParamsInstance)
					if
						not Result
						or (Result.Instance and Result.Instance.IsDescendantOf(Result.Instance, TargetCharacter))
					then
						return true
					end
				end
			end
			return false
		end

		local GetFirstVisibleBodyPart = function(Target, PartsList)
			local TargetCharacter = Target
			if Target and Target.IsA(Target, "Player") then
				TargetCharacter = Target.Character
			end
			if not TargetCharacter or not TargetCharacter.IsA(TargetCharacter, "Model") then
				return nil
			end
			local CameraPosition = Camera.CFrame.Position
			local RaycastParamsInstance = RaycastParams.new()
			RaycastParamsInstance.FilterType = Enum.RaycastFilterType.Exclude
			RaycastParamsInstance.FilterDescendantsInstances = { Players2.LocalPlayer.Character }
			for _, PartName in ipairs(PartsList) do
				local TargetPart = TargetCharacter.FindFirstChild(TargetCharacter, PartName)
				if TargetPart and TargetPart.IsA(TargetPart, "BasePart") then
					local Direction = TargetPart.Position - CameraPosition
					local Result = Workspace.Raycast(Workspace, CameraPosition, Direction, RaycastParamsInstance)
					if
						not Result
						or (Result.Instance and Result.Instance.IsDescendantOf(Result.Instance, TargetCharacter))
					then
						return TargetPart
					end
				end
			end
			return nil
		end

		GetClosestPlayer = function()
			local closest_entity, closest_d = nil, math.huge
			local all_targets = {}

			for _, player in Players2:GetPlayers() do
				if player ~= Players2.LocalPlayer and player.Character then
					table.insert(all_targets, player.Character)
				end
			end

			if Cheat.Options.SilentAim.TargetAI then
				for name, _ in pairs(SoldierClassType) do
					local ai = Workspace.AiZones:FindFirstChild(name)
					if ai and ai:FindFirstChild("HumanoidRootPart") then
						table.insert(all_targets, ai)
					end
				end
			end

			for _, character in ipairs(all_targets) do
				local rootPart = character:FindFirstChild("HumanoidRootPart")
				if rootPart then
					local targetPart

					if Cheat.Options.SilentAim.VisCheck then
						targetPart = GetFirstVisibleBodyPart(character, Cheat.Options.SilentAim.HitChance.List)
					else
						targetPart = character:FindFirstChild(Cheat.Options.SilentAim.HitPart) or rootPart
					end

					if targetPart then
						local partPosition = targetPart.Position
						local screenPoint, onScreen = Camera:WorldToViewportPoint(partPosition)

						if onScreen then
							local center = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
							local screen_pos = Vector2.new(screenPoint.X, screenPoint.Y)
							local distance = (screen_pos - center).Magnitude

							local isInFov = true
							if Cheat.Options.SilentAim.Fov.Use then
								isInFov = distance <= Cheat.Options.SilentAim.Fov.Size
							end

							if isInFov and distance < closest_d then
								closest_entity = character
								closest_d = distance
							end
						end
					end
				end
			end

			return closest_entity, closest_d
		end

		local function GetClosestHitPart(Character)
			local ClosestHitbox = nil
			local ShortestDistance = MathHuge

			local MousePosition = UserInputService:GetMouseLocation()
			local Hitboxes = {
				"Head",
				"HumanoidRootPart",
				"UpperTorso",
				"LowerTorso",
				"LeftUpperArm",
				"LeftLowerArm",
				"LeftHand",
				"RightUpperArm",
				"RightLowerArm",
				"RightHand",
				"LeftUpperLeg",
				"LeftLowerLeg",
				"LeftFoot",
				"RightUpperLeg",
				"RightLowerLeg",
				"RightFoot",
			}

			for _, HitboxName in IPairs(Hitboxes) do
				local Hitbox = Character:FindFirstChild(HitboxName)
				if Hitbox then
					local ScreenPoint, OnScreen = Camera:WorldToViewportPoint(Hitbox.Position)
					if OnScreen then
						local ScreenPosition = Vector2New(ScreenPoint.X, ScreenPoint.Y)
						local DistanceFromMouse = (ScreenPosition - MousePosition).Magnitude

						if DistanceFromMouse < ShortestDistance then
							ClosestHitbox = Hitbox
							ShortestDistance = DistanceFromMouse
						end
					end
				end
			end

			return ClosestHitbox
		end

		local function CalculateHitChance()
			return MathRandom(0, 100) <= Cheat.Options.SilentAim.HitChance.Amount
		end
		local kdr = ReplicatedStorage.Players
				and game.FindFirstChild(ReplicatedStorage.Players, Client.Name)
				and game.FindFirstChild(ReplicatedStorage.Players[Client.Name], "Status")
				and game.FindFirstChild(ReplicatedStorage.Players[Client.Name].Status, "Journey")
				and game.FindFirstChild(ReplicatedStorage.Players[Client.Name].Status.Journey, "Statistics")
				and ReplicatedStorage.Players[Client.Name].Status.Journey.Statistics:GetAttribute("Kills") .. "/" .. ReplicatedStorage.Players[Client.Name].Status.Journey.Statistics:GetAttribute(
					"Deaths"
				)
			or "0/0"
		LPH_NO_VIRTUALIZE(function()
			--
			do -- main
				Window = Menu:Create({ Name = "Niggahack", Size = Vector2.new(600, 500) })
				SkinEditor =
					Menu:Create({ Name = "Skin Editor", Size = Vector2.new(500, 400), PageAmmount = 1, Single = true })
				SkinEditor:Switch(false)
				do -- Extra
					Window:Watermark({
						Refresh = 100,
						Update = function()
							return "Niggahack | V2 | UID - 1 | KD - " .. kdr .. " | " .. os.date("%b %d %Y", os.time())
						end,
					})
					--
					--Window:Belt(Window)
					Window:Indicators(Window)
					Window:Belt(Window)
					--Window:Target(Window)
					Window:Armor(Window)
					Window:Switch(false)
				end
				--
				do -- ui setup
					do -- combat tab
						local CombatTab = Window:Page({ Name = "Combat" })
						--
						do -- sections
							Modifications, Combatmain, DrawingsSec, HitSec =
								CombatTab:Section({ Name = "Modifications", Side = "Right" }),
								CombatTab:Section({ Name = "Main" }),
								CombatTab:Section({ Name = "Drawings", Size = 50 }),
								CombatTab:Section({ Name = "Hit", Side = "Right", Size = 35 })
						end
						--
						do -- main / silent toggles
							local s = Cheat.Options.SilentAim
							Combatmain:Toggle({
								Name = "Silent Aim",
								Flag = "SilentAim",
								Callback = function(v)
									s.Enabled = v
									s.EnabledKB = v
									Flags["SilentHolder"]:Set(v)
								end,
							})
							Combatmain:Holder({
								Return = function(Self)
									Self:Dropdown({
										Flag = "Hitpart",
										Options = Cheat.Options.SilentAim.HitChance.List,
										Callback = function(v)
											s.HitChance.ClosestPart = (v == "Closest")
											if not s.HitChance.ClosestPart then
												s.HitPart = v
											end
										end,
									})
									Self:Toggle({
										Name = "Include AI",
										Flag = "targetai",
										Callback = function(v)
											s.TargetAI = v
										end,
									})
									Self:Toggle({
										Name = "Visible Check",
										Flag = "vischeck",
										Callback = function(v)
											s.VisCheck = v
										end,
									})
									--Self:Toggle({
									--	Name = "Prediction",
									--	Flag = "prediction",
									--	Callback = function(v)
									--		s.Prediction.Enabled = v
									--	end,
									--})
									Self:Slider({
										Flag = "Hitchance",
										Max = 100,
										Min = 1,
										Decimals = 1,
										State = 50,
										Suffix = "%",
										Callback = function(int)
											s.HitChance.Amount = int
										end,
									})
								end,
								Flag = "SilentHolder",
							})
							-- shit
							local underground = {
								enabled = false,
								bind = nil,
								resolver = false,
								resolvebind = nil,
								modelmanip = false,
							}

							local Players = game:GetService("Players")
							local Workspace = game:GetService("Workspace")
							local UserInputService = game:GetService("UserInputService")
							local RunService = game:GetService("RunService")

							local LocalPlayer = Players.LocalPlayer
							local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
							local HRP = Character:WaitForChild("HumanoidRootPart")

							local NotificationLifetime = 2.3
							local ResolveDepth = -30

							local desyncEnabled = false
							local desyncTable = {}
							local invistrack
							local invisAnim = Instance.new("Animation")
							invisAnim.AnimationId = "rbxassetid://15609995579"

							local Resolving = false
							local LoopingResolver = false
							local Velocity = 3650
							local Delay = 0.04
							local Offset = 10

							RunService.Heartbeat:Connect(function()
								if underground.enabled and underground.bind and Character and HRP then
									if not invistrack then
										invistrack = Character.Humanoid.Animator:LoadAnimation(invisAnim)
										invistrack:Play(0.01, 1, 0)
										invistrack.TimePosition = 2.35
									end

									desyncTable[1] = HRP.CFrame
									desyncTable[2] = HRP.AssemblyLinearVelocity

									local spoofedCF = HRP.CFrame
										* CFrame.new(0, -2.5, 0)
										* CFrame.Angles(math.rad(90), 0, 0)
									desyncTable[3] = spoofedCF

									HRP.CFrame = spoofedCF
									RunService.RenderStepped:Wait()
									HRP.CFrame = desyncTable[1]
									HRP.AssemblyLinearVelocity = desyncTable[2]
								else
								end
							end)

							local Players = game:GetService("Players")
							local Workspace = game:GetService("Workspace")
							local UserInputService = game:GetService("UserInputService")
							local runs = game:GetService("RunService")

							local localplayer = Players.LocalPlayer
							local Character = localplayer.Character or LocalPlayer.CharacterAdded:Wait()
							local HRP = Character:WaitForChild("HumanoidRootPart")

							local aimresolver = false
							local aimresolverpos = localplayer.Character.HumanoidRootPart.CFrame
							local aimresolvertime = 0
							local aimresolverbind = nil

							local uis = game:GetService("UserInputService")

							local function terrainresolve()
								if
									aimresolver
									and aimresolverbind
									and localplayer.Character
									and localplayer.Character.HumanoidRootPart
								then
									local char = localplayer.Character
									local hrp = char.HumanoidRootPart
									local mult = CFrame.new(0, -15, 0)
									if aimresolverhh then
										mult = CFrame.new(0, 500, 0)
									end
									hrp.AssemblyLinearVelocity = -mult.Position
									char:PivotTo(aimresolverpos * mult)
								else
									underground.bind = Flags["undergroundbind"]
								end
							end
							runs.Heartbeat:Connect(terrainresolve)
							local invisanim = Instance.new("Animation")
							invisanim.AnimationId = "rbxassetid://15609995579"

							modelmanip = false
							Combatmain:Toggle({
								Name = "Desync Resolver",
								Flag = "syncresolver",
								Callback = function(v)
									if v then
										local Players = game:GetService("Players")
										local LocalPlayer = Players.LocalPlayer
										local LocalCharacter = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

										local ReplicatedStorage = game:GetService("ReplicatedStorage")
										local PlayersData = ReplicatedStorage:WaitForChild("Players")
										local RunService = game:GetService("RunService")

										local connections = {}

										local function setupsync(Player, PlayerData)
											local Status = PlayerData:FindFirstChild("Status")
											if not Status then
												return
											end

											local function syncconnect(Character)
												local Root = Character:FindFirstChild("HumanoidRootPart")
												if not Root then
													return
												end

												if connections[Player.Name] then
													connections[Player.Name]:Disconnect()
												end

												local conn
												conn = RunService.Heartbeat:Connect(function()
													if not Players:FindFirstChild(Player.Name) or not PlayerData.Parent then
														if conn then
															conn:Disconnect()
														end
														connections[Player.Name] = nil
														return
													end

													local ServerPosition = Status:FindFirstChild("UAC")
														and Status.UAC:GetAttribute("LastVerifiedPos")
													if ServerPosition and Root and Root.Parent then
														Root.CFrame = CFrame.new(ServerPosition)
													end
												end)

												connections[Player.Name] = conn
											end

											if Player.Character then
												syncconnect(Player.Character)
											end

											Player.CharacterAdded:Connect(function(newChar)
												syncconnect(newChar)
											end)
										end

										for _, PlayerData in PlayersData:GetChildren() do
											local playerName = PlayerData.Name
											if playerName == LocalPlayer.Name then
												continue
											end

											local Player = Players:FindFirstChild(playerName)
											if not Player then
												continue
											end

											setupsync(Player, PlayerData)
										end

										Players.PlayerRemoving:Connect(function(leavingPlayer)
											local conn = connections[leavingPlayer.Name]
											if conn then
												conn:Disconnect()
												connections[leavingPlayer.Name] = nil
											end
										end)
									end
								end,
							})
							Combatmain:Toggle({
								Name = "Pitch Resolver",
								Flag = "pitchresolver",
								Callback = function(v)
									Cheat.Options.Misc.forcepitch = v
								end,
							})
							Combatmain:Toggle({
								Name = "Terrain Resolver",
								Flag = "terrainresolver",
								Callback = function(v)
									aimresolverpos = localplayer.Character.HumanoidRootPart.CFrame
									aimresolver = v
								end,
							}):Bind({
								Name = "Terrain Resolver",
								Flag = "trbind",
								Callback = function(v)
									aimresolverpos = localplayer.Character.HumanoidRootPart.CFrame
									aimresolverbind = v
									underground.bind = false
								end,
							})
							RunService = game:GetService("RunService")
							Players = game:GetService("Players")
							Workspace = game:GetService("Workspace")
							UserInputService = game:GetService("UserInputService")

							Client = Players.LocalPlayer
							Camera = Workspace.CurrentCamera
							RayParams = RaycastParams.new()
							RayParams.FilterDescendantsInstances = { Client.Character }
							RayParams.FilterType = Enum.RaycastFilterType.Blacklist

							local Visuals = {
								Bar = nil,
								Indicator = nil,
							}

							-- // funcs
							local function ispartvis(destination, origin)
								if not destination then
									return false
								end

								local raycastResult =
									Workspace:Raycast(origin, (destination.Position - origin).Unit * 10000, RayParams)
								if raycastResult and raycastResult.Instance then
									return raycastResult.Instance:IsDescendantOf(destination.Parent)
								end
								return false
							end

							local function findmanippos(origin, destination)
								local originalDestination = destination

								if ispartvis(originalDestination, origin) then
									State.Manipulated = false
									return origin
								end

								origin = typeof(origin) == "Vector3" and origin or origin.CFrame.p
								destination = typeof(destination) == "Vector3" and destination or destination.CFrame.p

								for index = 1, Manipulation.Distance do
									local potentialOrigins = {
										Vector3.new(origin.X + index, origin.Y, origin.Z),
										Vector3.new(origin.X, origin.Y + index, origin.Z),
										Vector3.new(origin.X, origin.Y, origin.Z + index),
										Vector3.new(origin.X - index, origin.Y, origin.Z),
										Vector3.new(origin.X, origin.Y - index, origin.Z),
										Vector3.new(origin.X, origin.Y, origin.Z - index),
									}

									for _, newOrigin in ipairs(potentialOrigins) do
										if ispartvis(originalDestination, newOrigin) then
											State.Manipulated = true
											State.ManipulatedIndex = index
											return newOrigin
										end
									end
								end

								State.Manipulated = false
								return origin
							end

							local function maniptarg()
								local closestPlayer = closestplr()
								if closestPlayer and closestPlayer.Character then
									local humanoidRootPart = closestPlayer.Character:FindFirstChild("HumanoidRootPart")
									local head = closestPlayer.Character:FindFirstChild("Head")

									if humanoidRootPart and head then
										State.TargetPart = head
										State.ManipulatedPosition = findmanippos(Client.Character.Head.Position, head)
										State.ManipulatedDirection = head.Position
									end
								else
									State.TargetPart = nil
								end
							end

							-- // hooking
							local namecallHook
							namecallHook = hookmetamethod(game, "__namecall", function(self, ...)
								local args = { ... }
								local method = getnamecallmethod()

								if not checkcaller() and method == "Raycast" then
									local traceback = debug.traceback()
									if Manipulation.Enabled and traceback and traceback:find("onStep", 1, true) then
										if State.ManipulatedPosition and State.TargetPart and State.IsShooting then
											args[1] = State.ManipulatedPosition
											args[2] = (State.TargetPart.Position - State.ManipulatedPosition).Unit
										end
									end
								end
								return namecallHook(self, unpack(args))
							end)

							local manipulationLabel = Instance.new("TextLabel")
							manipulationLabel.Size = UDim2.new(0, 200, 0, 50)
							manipulationLabel.Position = UDim2.new(0.5, -100, 0.55, 0)
							manipulationLabel.BackgroundTransparency = 1
							manipulationLabel.Text = "Manipulated"
							manipulationLabel.FontFace = Visuals2.Fonts["SmallestPixel"][1]
							manipulationLabel.TextSize = 9
							manipulationLabel.TextColor3 = Color3.new(255, 255, 255)
							manipulationLabel.TextScaled = true
							manipulationLabel.Visible = false
							manipulationLabel.Parent = game.Players.LocalPlayer
								:WaitForChild("PlayerGui")
								:FindFirstChild("ScreenGui") or Instance.new(
								"ScreenGui",
								game.Players.LocalPlayer:WaitForChild("PlayerGui")
							)

							local manipulationLoop
							manipulationLoop = RunService.Heartbeat:Connect(function()
								if not Manipulation.Enabled then
									return
								end
								if Client.Character and Client.Character:FindFirstChild("HumanoidRootPart") then
									local root = Client.Character.HumanoidRootPart
									Fake.Real.CFrame = root.CFrame

									if State.ManipulatedPosition and State.ManipulatedDirection and State.TargetPart then
										if State.Manipulated and State.IsShooting then
											root.CFrame = CFrame.new(State.ManipulatedPosition, State.ManipulatedDirection)
											Fake.Fake.CFrame = Client.Character.HumanoidRootPart.CFrame
										end
									end
								end
							end)

							forcehitmain = {
								enabled = false,
								bind = false,
							}

							local Players = game:GetService("Players")
							local LocalPlayer = Players.LocalPlayer
							local Camera = workspace:FindFirstChildOfClass("Camera")

							local function get_closest_target()
								if not Camera then
									Camera = workspace:FindFirstChildOfClass("Camera")
									if not Camera then
										return nil
									end
								end
								local target_part = nil
								local max_distance = math.huge
								local viewport = Camera.ViewportSize
								local mouse_pos = Vector2.new(viewport.X / 2, viewport.Y / 2)
								for _, player in Players:GetPlayers() do
									if player ~= LocalPlayer and player.Character then
										local part = player.Character:FindFirstChild(Cheat.Options.SilentAim.HitPart)
										local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
										if part and humanoid and humanoid.Health > 0 then
											local ok, screen_pos, on_screen = pcall(function()
												return Camera:WorldToViewportPoint(part.Position)
											end)
											if ok and on_screen then
												local distance = (Vector2.new(screen_pos.X, screen_pos.Y) - mouse_pos).Magnitude
												if distance < max_distance then
													target_part = part
													max_distance = distance
												end
											end
										end
									end
								end
								return target_part
							end

							local __namecall
							__namecall = hookmetamethod(game, "__namecall", function(self, ...)
								local method = getnamecallmethod()
								local args = { ... }
								if
									not checkcaller()
									and forcehitmain.enabled
									and forcehitmain.bind
									and method == "Raycast"
								then
									local origin = args[1]
									if typeof(origin) == "Vector3" then
										local target_part = get_closest_target()
										if target_part then
											return {
												Instance = target_part,
												Position = target_part.Position,
												Normal = (target_part.Position - origin).Unit,
												Material = target_part.Material or Enum.Material.Plastic,
												Distance = (target_part.Position - origin).Magnitude,
											}
										end
									end
								end
								return __namecall(self, ...)
							end)

							Combatmain:Toggle({
								Name = "Manipulation",
								Flag = "Manipulation",
								Callback = function(state)
									Manipulation.Enabled = state
									forcehitmain.enabled = state
								end,
							}):Bind({
								Flag = "Manipulation",
								Mode = "On Hold",
								Callback = function(state)
									Manipulation.Bind = state
									forcehitmain.bind = state
									if Manipulation.Enabled then
										maniptarg()
									end
								end,
							})
							Combatmain:Slider({
								Flag = "Manipulation Distance",
								Max = 15,
								Min = 1,
								Default = 15,
								Suffix = "",
								Callback = function(state)
									Manipulation.Distance = state
								end,
							})
						end
						--
						do -- drawing toggles
							local d = DrawingsSec
							local f = Cheat.Options.SilentAim.Fov
							local a = d:Toggle({
								Name = "FOV Circle",
								Flag = "drawfov",
								Callback = function(v)
									Flags["FOVHolder"]:Set(v)
									f.Use = v
									f.Visible = v
									f.Outline = v
								end,
							})
							d:Slider({
								Flag = "Radius",
								Max = 1000,
								Min = 10,
								Callback = function(int)
									f.Size = int
								end,
							})
							a:Picker({
								Flag = "fovclr1",
								Alpha = 0,
								Callback = function(c, t)
									f.Gradient_clr1 = c
									f.InTrans = t
								end,
							})
							a:Picker({
								Flag = "fovclr2",
								Alpha = 0,
								Callback = function(c, t)
									f.Gradient_clr2 = c
									f.OutTrans = t
								end,
							})
							d:Holder({
								Return = function(Self)
									Self:Dropdown({
										Flag = "fovstyle",
										Options = { "Normal", "Dynamic" },
										Callback = function(v)
											f.Type = v
										end,
									})
									Self:Toggle({
										Name = "Rotate",
										Flag = "rotatefov",
										Callback = function(v)
											f.Gradient_rotate = v
											Flags["RotateHolder"]:Set(v)
										end,
									})
									Self:Holder({
										Return = function(Self1)
											Self1:Slider({
												Flag = "RotateSpeed",
												Max = 200,
												Min = 0,
												Callback = function(int)
													f.Rotation_speed = int
												end,
											})
										end,
										Flag = "RotateHolder",
									})
									Self:Dropdown({
										Flag = "fovpos",
										Options = { "Mouse", "Middle", "Barrel" },
										Callback = function(v)
											f.Position = v
										end,
									})
								end,
								Flag = "FOVHolder",
							})
						end
						--
						do -- weapon toggles
							local mods = Cheat.Options.GunMods
							local rp = game:GetService("ReplicatedStorage")
							local realAmmoTypes = rp:FindFirstChild("realAmmoTypes")
								or rp:FindFirstChild("AmmoTypes") and rp:FindFirstChild("AmmoTypes"):Clone()
							if realAmmoTypes then
								realAmmoTypes.Name = "realAmmoTypes"
							end

							local cheat = {
								connections = {
									heartbeats = {},
									renderstepped = {},
								},
								drawings = {},
								hooks = {},
							}

							cheat.utility = {}
							gunMods = {}
							rpd = 0
							local function firerateShit()
								for i, gc in next, getgc(true) do
									if type(gc) == "table" then
										if rawget(gc, "updateClient") then
											local old_update = gc.updateClient
											gc.updateClient = function(...)
												local args = { ... }
												if gunMods.rapidfire then
													if gunMods.rapidfire then
														args[1].FireRate = rpd
													end
													args[1].FireMode = "Auto"
												end
												return old_update(unpack(args))
											end
										end
									end
								end
							end
							firerateShit()
							do -- dumb recoil
								local function recoilshit()
									for i, gc in next, getgc(true) do
										if type(gc) == "table" then
											if rawget(gc, "shove") and rawget(gc, "update") then
												local shove, update = gc.shove, gc.update
												gc.shove = function(self, recoil)
													local scale = (mods.RecoilVal or 100) / 100
													if typeof(recoil) == "Vector3" then
														recoil = recoil * scale
													end
													return shove(self, recoil)
												end
											end

											if
												type(rawget(gc, "create")) == "function"
												and getinfo(gc.create).short_src == "ReplicatedStorage.Modules.SpringV2"
											then
												local old_create = gc.create
												gc.create = function(...)
													local returns = old_create(...)
													local shove, update = returns.shove, returns.update
													returns.shove = function(self, recoil)
														local scale = (mods.RecoilVal or 100) / 100
														if typeof(recoil) == "Vector3" then
															recoil = recoil * scale
														end
														return shove(self, recoil)
													end
													return returns
												end
											end
										end
									end
								end
								recoilshit()
							end
							Modifications:Label({ Name = "Recoil" })
							Modifications:Slider({
								Flag = "RecoilAmount",
								Max = 100,
								Min = 0,
								State = 100,
								Suffix = "%",
								Callback = function(int)
									mods.RecoilVal = int
								end,
							})
							Modifications:Toggle({
								Name = "Remove Tracer",
								Flag = "NoTracer",
								Callback = function(v)
									if v and rp:FindFirstChild("AmmoTypes") then
										for i, v in ipairs(rp.AmmoTypes:GetChildren()) do
											if v then
												v:SetAttribute("Tracer", false)
											end
										end
									elseif not v and rp:FindFirstChild("AmmoTypes") then
										for i, v in ipairs(rp.AmmoTypes:GetChildren()) do
											if v then
												local realAmmo = realAmmoTypes:FindFirstChild(v.Name)
												if realAmmo then
													v:SetAttribute("Tracer", realAmmo:GetAttribute("Tracer"))
												end
											end
										end
									end
								end,
							})
							Modifications:Toggle({
								Name = "Instant Equip",
								Flag = "instantequip",
								Callback = function(v)
									mods.InstantEquip.Value = v
								end,
							})
							Modifications:Toggle({
								Name = "Instant Aim",
								Flag = "instantaim",
								Callback = function(v)
									mods.InstantAim.Value = v
								end,
							})
							Modifications:Toggle({
								Name = "Remove Obstructions",
								Flag = "removeobs",
								Callback = function(v)
									mods.RemoveObstructions.Value = v
								end,
							})
							Modifications:Toggle({
								Name = "Remove Spread",
								Flag = "removespread",
								Callback = function(v)
									local ammo = game.ReplicatedStorage:FindFirstChild("AmmoTypes")
									if ammo then
										for _, v in pairs(ammo:GetChildren()) do
											v:SetAttribute("ProjectileWidth", first and "0" or "DefaultWidth")
										end
									else
										warn("ammotype don een exist LMFAOO")
									end
								end,
							})
							Modifications:Toggle({
								Name = "Double Tap",
								Flag = "dt",
								Callback = function(v)
									mods.doubleTap = v
								end,
							})
							Modifications:Toggle({
								Name = "Instant Hit",
								Flag = "instanthit",
								Callback = function(v)
									mods.InstaBullet = v
								end,
							})
							Modifications:Toggle({
								Name = "Unlock Firemodes",
								Flag = "forceauto",
								Callback = function(v)
									mods.UnlockFiremodes.Value = v
								end,
							})
							Modifications:Toggle({
								Name = "Instant Lean",
								Flag = "instantlean",
								Callback = function(v)
									mods.InstantLean.Value = v
								end,
							})
							Modifications:Toggle({
								Name = "Rapidfire",
								Flag = "rapidfire",
								Callback = function(v)
									Flags["RapidFireHolder"]:Set(v)
									gunMods.rapidfire = v
								end,
							})
							Modifications:Holder({
								Return = function(Self)
									Self:Label({ Name = "Delay", Inlayed = true })
									Self:Slider({
										Name = "Firerate",
										Flag = "Firerate",
										Max = 1,
										Min = 0,
										Decimals = 0.01,
										Callback = function(int)
											rpd = int
										end,
									})
								end,
								Flag = "RapidFireHolder",
							})
							Modifications:Toggle({
								Name = "Always Headshot",
								Flag = "AlwaysHeadshot",
								Callback = function(v)
									mods.AlwaysHeadshot = v
								end,
							})
							HitSec:Toggle({
								Name = "Hitmarkers",
								Flag = "hitmarkers",
								Callback = function(v)
									Cheat.Options.Misc.Weapon.Hitmarkers = v
								end,
							}):Picker({
								Flag = "hmc",
								Callback = function(c)
									Cheat.Options.Misc.Weapon.HitmarkerColor = c
								end,
							})
							HitSec:Toggle({
								Name = "Damage Numbers",
								Flag = "Damage Numbers",
								Callback = function(v)
									Cheat.Options.Misc.Weapon.DamageNumber = v
								end,
							}):Picker({
								Flag = "dnc",
								Callback = function(c)
									Cheat.Options.Misc.Weapon.DamageColor = c
								end,
							})
							HitSec:Toggle({
								Name = "Hit Effects",
								Flag = "Hit Effects",
								Callback = function(v)
									Flags["EffectHolder"]:Set(v)
									Cheat.Options.Misc.Weapon.EffectEnabled = v
								end,
							}):Picker({
								Flag = "hec",
								State = Color3.fromRGB(255, 255, 255),
								Callback = function(c)
									Cheat.Options.Misc.Weapon.EffectColor = c
								end,
							})
							HitSec:Holder({
								Return = function(Self)
									local m = Cheat.Options.Misc.Weapon
									Self:Dropdown({
										Flag = "hiteffects",
										Options = {
											"Blackhole",
											"Bubble",
											"Flame",
											"Glow",
											"Ground Spiral",
											"Impact",
											"KO",
											"MLG",
											"Shock Bubble",
											"Shockwave",
											"Shockwave Explosion",
											"Slash",
											"Soul Slash",
											"Sparkles",
											"Sparks",
											"Spiral",
											"Spiral Slash",
											"Shine",
											"Summon",
											"Supernova",
											"Tornado",
											"Zzz",
										},
										Callback = function(v)
											m.Effect = v
										end,
									})
									Self:Slider({
										Flag = "EffectTime",
										Max = 3,
										Min = 0,
										Decimals = 0.1,
										State = 1,
										Callback = function(int)
											m.EffectTime = int
										end,
									})
								end,
								Flag = "EffectHolder",
							})
						end
					end
					--
					do -- visuals tab
						local VisualsTab = Window:Page({ Name = "Visuals" })
						--
						local sections = {
							Lighting = VisualsTab:Section({ Name = "Lighting", Size = 40 }),
							Self = VisualsTab:Section({ Name = "Self", Side = "Right", Size = 50 }),
						}
						sections.Self:Toggle({
							Name = "Aspect Ratio",
							Flag = "AspectRatio",
							Callback = function(v)
								Cheat.Options.Misc.AspectRatio.Enabled = v
							end,
						})
						sections.Self:Slider({
							Flag = "AspectRatioX",
							Min = 0,
							Max = 100,
							Default = 100,
							Callback = function(int)
								Cheat.Options.Misc.AspectRatio.Horizontal = int
							end,
						})
						sections.Self:Slider({
							Flag = "AspectRatioY",
							Min = 0,
							Max = 100,
							Default = 100,
							Callback = function(int)
								Cheat.Options.Misc.AspectRatio.Vertical = int
							end,
						})
					end
					--
					do -- esp tab
						local EspTab = Window:Page({ Name = "ESP" })
						--
						do -- funcs
							local Players = game:GetService("Players")
							local RunService = game:GetService("RunService")
							local Workspace = game:GetService("Workspace")
							local HttpService = game:GetService("HttpService")
							local Camera = Workspace.CurrentCamera
							local LocalPlayer = Players.LocalPlayer
							local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

							local istestinggame = workspace:FindFirstChild("HealthRigJolt")

							_G.espCFG = {
								Enabled = true,
								TeamCheck = false,
								FPS = 60,
								Players = {
									Enabled = true,
									MaxDistance = 3000,

									Box = {
										Enabled = true,

										Boxes = true,
										Type = "Full", -- Full / Corners
										Color = Color3.fromRGB(180, 220, 255),
										Transparency = 0,

										OutlineColor = Color3.fromRGB(25, 25, 30),
										OutlineTransparency = 0.15,
										InnerOutlineTransparency = 0.15,

										-- filled bogus!
										Filled = false,
										FilledColor1 = Color3.fromRGB(50, 120, 255),
										FilledTransparency1 = 1,

										FilledColor2 = Color3.fromRGB(120, 0, 255),
										FilledTransparency2 = 0,

										FilledRotation = 135,
									},

									Chams = {
										Enabled = false,
										NormalColor = { Color = Color3.fromRGB(80, 255, 140), Transparency = 0.4 },
										GlowColor = { Color = Color3.fromRGB(100, 200, 255), Transparency = 0.1 },
									},

									Health = {
										Enabled = true,
										Pos = "Left",
										GradientLow = Color3.fromRGB(255, 70, 70),
										GradientMid = Color3.fromRGB(255, 210, 90),
										GradientHigh = Color3.fromRGB(90, 255, 100),
										Thickness = 0.1,
									},

									Name = {
										Enabled = true,
										Color = Color3.fromRGB(240, 240, 255),
										Transparency = 0,
										Pos = "Top",
										UseDisplayName = true,
									},

									Distance = {
										Enabled = true,
										Color = Color3.fromRGB(160, 200, 255),
										Transparency = 0,
										Pos = "Bottom",
									},

									Weapon = {
										Enabled = true,
										Color = Color3.fromRGB(255, 220, 150),
										Transparency = 0,
										Pos = "Right",
									},

									WeaponIcon = {
										Enabled = false,
										Pos = "Top",
									},

									Flags = {
										Enabled = true,
										Pos = "Top",
										EnabledFlags = {
											Aiming = true,
											Healing = true,
											VIP = true,
										},
										FlagsColors = {
											Aiming = { Color = Color3.fromRGB(255, 100, 100), Transparency = 0 },
											Healing = { Color = Color3.fromRGB(80, 255, 150), Transparency = 0 },
											VIP = { Color = Color3.fromRGB(100, 200, 255), Transparency = 0 },
										},
									},
									Arrows = {
										Enabled = false,
										Point = "Three", -- Three, Four
										ShowDistance = false,
										Clr = Color3.fromRGB(255, 255, 255),
										Transparency = 0,
										Size = 22,
										Radius = 300,
										Position = "Middle", -- Middle, Barrel
									},
								},
								Soldiers = {
									Enabled = true,
									MaxDistance = 3000,

									Box = {
										Enabled = false,

										Boxes = true,
										Type = "Corners", -- Full / Corners
										Color = Color3.fromRGB(180, 220, 255),
										Transparency = 0,

										OutlineColor = Color3.fromRGB(25, 25, 30),
										OutlineTransparency = 0.15,
										InnerOutlineTransparency = 0.15,

										-- filled bogus!
										Filled = true,
										FilledColor1 = Color3.fromRGB(50, 120, 255),
										FilledTransparency1 = 0.9,

										FilledColor2 = Color3.fromRGB(120, 0, 255),
										FilledTransparency2 = 0.15,

										FilledRotation = 135,
									},

									Chams = {
										Enabled = true,
										NormalColor = { Color = Color3.fromRGB(80, 255, 140), Transparency = 0.8 },
										GlowColor = { Color = Color3.fromRGB(100, 200, 255), Transparency = 0.6 },
									},

									Health = {
										Enabled = true,
										Pos = "top",
										GradientLow = Color3.fromRGB(255, 70, 70),
										GradientMid = Color3.fromRGB(255, 210, 90),
										GradientHigh = Color3.fromRGB(90, 255, 100),
										Thickness = 1,
									},

									Name = {
										Enabled = true,
										Color = Color3.fromRGB(240, 240, 255),
										Transparency = 0,
										Pos = "Top",
										UseDisplayName = true,
									},

									Distance = {
										Enabled = true,
										Color = Color3.fromRGB(160, 200, 255),
										Transparency = 0,
										Pos = "Bottom",
									},

									Weapon = {
										Enabled = false,
										Color = Color3.fromRGB(255, 220, 150),
										Transparency = 0,
										Pos = "Right",
									},

									WeaponIcon = {
										Enabled = false,
										Pos = "Bottom",
									},

									Flags = {
										Enabled = false,
										Pos = "Right",
										EnabledFlags = {
											Aiming = true,
											Healing = true,
											VIP = true,
										},
										FlagsColors = {
											Aiming = { Color = Color3.fromRGB(255, 100, 100), Transparency = 0 },
											Healing = { Color = Color3.fromRGB(80, 255, 150), Transparency = 0 },
											VIP = { Color = Color3.fromRGB(100, 200, 255), Transparency = 0 },
										},
									},
								},
								Entites = {
									Airdrop = {
										Enabled = false,
										Clr = Color3.fromRGB(255, 255, 255),
										Transparency = 0,
									},
									Corpses = {
										Enabled = false,
										Clr = Color3.fromRGB(255, 255, 255),
										Transparency = 0,
									},
									Extract = {
										Enabled = false,
										Clr = Color3.fromRGB(255, 255, 255),
										Transparency = 0,
									},
								},
							}

							local Cache = {}
							--Globals.ItemsModule = require(game:GetService("ReplicatedStorage").Modules.Items)
							Globals.IconsCache = {}

							local Holder = Instance.new("ScreenGui")
							Holder.IgnoreGuiInset = true
							Holder.Parent = gethui()

							local function BoxMath(model)
								if not model then
									return nil, nil, false
								end
								local root = model:FindFirstChild("HumanoidRootPart")
								if not root then
									return nil, nil, false
								end

								local hrpPos, onScreen = Camera:WorldToViewportPoint(root.Position)
								if not onScreen or hrpPos.Z <= 0 then
									return nil, nil, false
								end

								local headPos, onScreen =
									Camera:WorldToViewportPoint(root.Position + Vector3.new(0, 2.5, 0))
								local footPos, onScreen2 =
									Camera:WorldToViewportPoint(root.Position - Vector3.new(0, 3.5, 0))
								local height = math.abs(headPos.Y - footPos.Y)
								local width = height / 1.3
								local boxPos = Vector2.new(hrpPos.X - width / 2, headPos.Y)
								local boxSize = Vector2.new(width, height)

								return boxPos, boxSize, true, width, height
							end

							local function resolveGun(name)
								if Globals.IconsCache[name] then
									return Globals.IconsCache[name]
								end
								return false
							end

							local textfont = Visuals2.Fonts["SmallestPixel"][1]
							local ItemCache = {}

							local HealingAnimationIds = {
								["rbxassetid://14860445359"] = true,
								["rbxassetid://2529243689"] = true,
							}
							aimingAnims = 12698505006
								or 7870350007
								or 10480133718
								or 11764413979
								or 87787940852962
								or 13115129333
								or 10480133718
								or 10352537863
								or 11764413979
								or 12698505006
								or 11764413979
								or 11764413979
								or 10916393728
								or 10480133718
								or 10008742871
								or 11715008658
								or 10480133718
								or 10661085615
								or 10480133718
								or 11764413979
								or 15252577195
								or 10916393728
								or 10918888537
							local ESPFlags = {
								Aiming = {
									Text = function(player)
										return "Aiming"
									end,
									Allow = function(player)
										if not player:IsA("Player") then
											return false
										end

										local Character = player.Character
										if Character then
											return false
										end

										local hum = Character:FindFirstChild("Humanoid")
										if not hum then
											return false
										end

										local Root = hum.RootPart
										if not Root then
											return false
										end

										for _, track in hum:GetPlayingAnimationTracks() do
											if track.Animation.AnimationId == aimingAnims then
												return true
											end
										end

										return false
									end,
								},
								Healing = {
									Text = function(player)
										return "Healing"
									end,
									Allow = function(player)
										if player.Name == "HealingRig" then
											return true
										end
										if not player:IsA("Player") then
											return false
										end
										local Character = player.Character

										if not Character then
											return false
										end

										local hum = Character:FindFirstChild("Humanoid")
										if not hum then
											return false
										end

										for _, track in hum:GetPlayingAnimationTracks() do
											if HealingAnimationIds[track.Animation.AnimationId] then
												return true
											end
										end
										return false
									end,
								},
								VIP = {
									Text = function(player)
										return "VIP"
									end,
									Allow = function(player)
										if player.Name == "VIPRig" then
											return true
										end
										if not player:IsA("Player") then
											return false
										end

										return player and player:GetAttribute("VIP")
									end,
								},
							}

							local Workspace = game:GetService("Workspace")
							local AiZones = Workspace:WaitForChild("AiZones")

							for _, v in pairs(AiZones:GetDescendants()) do
								if v:FindFirstChild("Humanoid") then
									SoldierClassType[v.Name] = "Soldier"
								end
							end

							AiZones.DescendantAdded:Connect(function(Child)
								task.wait(1)
								if Child:FindFirstChild("Humanoid") then
									SoldierClassType[Child.Name] = "Soldier"
								end
							end)

							local function Espify(plr)
								if plr == LocalPlayer or Cache[plr] then
									return
								end
								--
								local frame = Instance.new("Frame")
								frame.BackgroundColor3 = Color3.new(1, 1, 1)
								frame.BorderColor3 = Color3.new()
								frame.BorderSizePixel = 0
								frame.Position = UDim2.fromScale(0.44166, 0.300347)
								frame.Size = UDim2.fromOffset(298, 459)
								frame.Visible = false

								local uIGradient = Instance.new("UIGradient")
								uIGradient.Color = ColorSequence.new({
									ColorSequenceKeypoint.new(0, Color3.fromRGB(100, 100, 100)),
									ColorSequenceKeypoint.new(1, Color3.new(1, 1, 1)),
								})
								uIGradient.Rotation = 27
								uIGradient.Transparency = NumberSequence.new({
									NumberSequenceKeypoint.new(0, 0.5),
									NumberSequenceKeypoint.new(1, 0.5),
								})
								uIGradient.Parent = frame

								local box = Instance.new("Frame")
								box.BackgroundTransparency = 1
								box.Size = UDim2.fromScale(1, 1)

								local mainOutline = Instance.new("Frame")
								mainOutline.BackgroundTransparency = 1
								mainOutline.Size = UDim2.fromScale(1, 1)

								local uIStroke = Instance.new("UIStroke")
								uIStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
								uIStroke.LineJoinMode = Enum.LineJoinMode.Miter
								uIStroke.Thickness = 3
								uIStroke.Parent = mainOutline

								mainOutline.Parent = box

								local mainBox = Instance.new("Frame")
								mainBox.BackgroundTransparency = 1
								mainBox.Size = UDim2.fromScale(1, 1)

								local uIStroke1 = Instance.new("UIStroke")
								uIStroke1.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
								uIStroke1.Color = Color3.new(1, 1, 1)
								uIStroke1.LineJoinMode = Enum.LineJoinMode.Miter
								uIStroke1.Thickness = 2
								uIStroke1.Parent = mainBox

								mainBox.Parent = box

								local innerOutline = Instance.new("Frame")
								innerOutline.BackgroundTransparency = 1
								innerOutline.Size = UDim2.fromScale(1, 1)

								local uIStroke2 = Instance.new("UIStroke")
								uIStroke2.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
								uIStroke2.LineJoinMode = Enum.LineJoinMode.Miter
								uIStroke2.Parent = innerOutline

								innerOutline.Parent = box

								box.Parent = frame

								local cornerHolder = Instance.new("Frame")
								cornerHolder.Size = UDim2.new(1, 0, 1, 0)
								cornerHolder.BackgroundTransparency = 1
								cornerHolder.Visible = false
								cornerHolder.ZIndex = 1
								cornerHolder.Parent = frame

								local corners = {}
								for _, pos in next, { { 0, 0 }, { 1, 0 }, { 0, 1 }, { 1, 1 } } do
									local len = 15
									local thick = 1

									local horiz = Instance.new("Frame")
									horiz.AnchorPoint = Vector2.new(pos[1], pos[2])
									horiz.Position = UDim2.new(pos[1], 0, pos[2], 0)
									horiz.Size = UDim2.new(0, len, 0, thick)
									horiz.BorderSizePixel = 0
									horiz.ZIndex = 2
									horiz.Parent = cornerHolder

									local horizoutline = Instance.new("Frame")
									horizoutline.BackgroundColor3 = Color3.new()
									horizoutline.Size = UDim2.new(1, 2, 1, 2)
									horizoutline.Position = UDim2.new(0, -1, 0, -1)
									horizoutline.BorderSizePixel = 0
									horizoutline.ZIndex = -999
									horizoutline.Parent = horiz

									local vert = Instance.new("Frame")
									vert.AnchorPoint = Vector2.new(pos[1], pos[2])
									vert.Position = UDim2.new(pos[1], 0, pos[2], 0)
									vert.Size = UDim2.new(0, thick, 0, len)
									vert.BorderSizePixel = 0
									vert.ZIndex = 2
									vert.Parent = cornerHolder

									local vertoutline = Instance.new("Frame")
									vertoutline.BackgroundColor3 = Color3.new()
									vertoutline.Size = UDim2.new(1, 2, 1, 2)
									vertoutline.Position = UDim2.new(0, -1, 0, -1)
									vertoutline.BorderSizePixel = 0
									vertoutline.ZIndex = -999
									vertoutline.Parent = vert

									table.insert(corners, { horiz, vertoutline })
									table.insert(corners, { vert, horizoutline })
								end

								local sides = Instance.new("Frame")
								sides.BackgroundTransparency = 1
								sides.Size = UDim2.fromScale(1, 1)

								local bottom = Instance.new("Frame")
								bottom.BackgroundTransparency = 1
								bottom.Position = UDim2.new(0, 0, 1, 5)
								bottom.Size = UDim2.fromScale(1, 1)

								local objects = Instance.new("Frame")
								objects.AutomaticSize = Enum.AutomaticSize.XY
								objects.BackgroundTransparency = 1

								local uIListLayout = Instance.new("UIListLayout")
								uIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
								uIListLayout.ItemLineAlignment = Enum.ItemLineAlignment.Center
								uIListLayout.Padding = UDim.new(0, 5)
								uIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
								uIListLayout.Parent = objects

								objects.Parent = bottom

								local uIListLayout1 = Instance.new("UIListLayout")
								uIListLayout1.HorizontalAlignment = Enum.HorizontalAlignment.Center
								uIListLayout1.ItemLineAlignment = Enum.ItemLineAlignment.Center
								uIListLayout1.Padding = UDim.new(0, 5)
								uIListLayout1.SortOrder = Enum.SortOrder.LayoutOrder
								uIListLayout1.Parent = bottom

								bottom.Parent = sides

								local right = Instance.new("Frame")
								right.BackgroundTransparency = 1
								right.Position = UDim2.new(1, 8, 0, 0)
								right.Size = UDim2.fromScale(1, 1)

								local objects1 = Instance.new("Frame")
								objects1.AutomaticSize = Enum.AutomaticSize.XY
								objects1.BackgroundTransparency = 1

								local uIListLayout2 = Instance.new("UIListLayout")
								uIListLayout2.ItemLineAlignment = Enum.ItemLineAlignment.Start
								uIListLayout2.Padding = UDim.new(0, 5)
								uIListLayout2.SortOrder = Enum.SortOrder.LayoutOrder
								uIListLayout2.Parent = objects1

								objects1.Parent = right

								local uIListLayout3 = Instance.new("UIListLayout")
								uIListLayout3.FillDirection = Enum.FillDirection.Horizontal
								uIListLayout3.Padding = UDim.new(0, 5)
								uIListLayout3.SortOrder = Enum.SortOrder.LayoutOrder
								uIListLayout3.Parent = right

								local healthbar = Instance.new("Frame")
								healthbar.BackgroundColor3 = Color3.new(1, 1, 1)
								healthbar.BorderColor3 = Color3.new()
								healthbar.BorderSizePixel = 0
								healthbar.LayoutOrder = -100
								healthbar.AnchorPoint = Vector2.new(0, 0)
								healthbar.Size = UDim2.new(0, 2, 1, 1)

								local uIGradient1 = Instance.new("UIGradient")
								uIGradient1.Color = ColorSequence.new({
									ColorSequenceKeypoint.new(0, Color3.fromRGB(200, 0, 0)),
									ColorSequenceKeypoint.new(0.5, Color3.fromRGB(200, 200, 0)),
									ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 200, 0)),
								})
								uIGradient1.Rotation = -90
								uIGradient1.Parent = healthbar

								local uIStroke3 = Instance.new("UIStroke")
								uIStroke3.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
								uIStroke3.Parent = healthbar

								local healthbarOutline = Instance.new("Frame")
								healthbarOutline.BackgroundColor3 = Color3.fromRGB(36, 36, 36)
								healthbarOutline.BorderColor3 = Color3.new()
								healthbarOutline.BorderSizePixel = 0
								healthbarOutline.Size = UDim2.fromScale(1, 0.31)
								healthbarOutline.Parent = healthbar

								healthbar.Parent = right

								right.Parent = sides

								local top = Instance.new("Frame")
								top.AnchorPoint = Vector2.new(0, 1)
								top.BackgroundTransparency = 1
								top.Position = UDim2.fromOffset(0, -8)
								top.Size = UDim2.fromScale(1, 1)

								local objects2 = Instance.new("Frame")
								objects2.AnchorPoint = Vector2.new(0, 1)
								objects2.AutomaticSize = Enum.AutomaticSize.XY
								objects2.BackgroundTransparency = 1
								objects2.Position = UDim2.fromScale(0, 1)

								local uIListLayout4 = Instance.new("UIListLayout")
								uIListLayout4.HorizontalAlignment = Enum.HorizontalAlignment.Center
								uIListLayout4.ItemLineAlignment = Enum.ItemLineAlignment.Center
								uIListLayout4.Padding = UDim.new(0, 5)
								uIListLayout4.SortOrder = Enum.SortOrder.LayoutOrder
								uIListLayout4.VerticalAlignment = Enum.VerticalAlignment.Bottom
								uIListLayout4.Parent = objects2

								objects2.Parent = top

								local uIListLayout5 = Instance.new("UIListLayout")
								uIListLayout5.HorizontalAlignment = Enum.HorizontalAlignment.Center
								uIListLayout5.ItemLineAlignment = Enum.ItemLineAlignment.Center
								uIListLayout5.Padding = UDim.new(0, 5)
								uIListLayout5.SortOrder = Enum.SortOrder.LayoutOrder
								uIListLayout5.VerticalAlignment = Enum.VerticalAlignment.Bottom
								uIListLayout5.Parent = top

								local horizontalHealthBar = Instance.new("Frame")
								horizontalHealthBar.BackgroundColor3 = Color3.new(1, 1, 1)
								horizontalHealthBar.BorderColor3 = Color3.new()
								horizontalHealthBar.BorderSizePixel = 0
								horizontalHealthBar.LayoutOrder = -100
								horizontalHealthBar.AnchorPoint = Vector2.new(0, 0)
								horizontalHealthBar.Size = UDim2.new(1, 0, 0, 2)

								local uIGradient2 = Instance.new("UIGradient")
								uIGradient2.Color = ColorSequence.new({
									ColorSequenceKeypoint.new(0, Color3.fromRGB(200, 0, 0)),
									ColorSequenceKeypoint.new(0.5, Color3.fromRGB(200, 200, 0)),
									ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 200, 0)),
								})
								uIGradient2.Rotation = 180
								uIGradient2.Parent = horizontalHealthBar

								local stupidshit = Instance.new("Frame")
								stupidshit.BackgroundColor3 = Color3.fromRGB(36, 36, 36)
								stupidshit.BorderColor3 = Color3.new()
								stupidshit.BorderSizePixel = 0
								stupidshit.Position = UDim2.new(0, 0, 0, -1)
								stupidshit.Size = UDim2.new(1, 2, 1, 2)
								stupidshit.ZIndex = -10
								stupidshit.Parent = horizontalHealthBar

								local healthbarOutline1 = Instance.new("Frame")
								healthbarOutline1.AnchorPoint = Vector2.new(1, 0)
								healthbarOutline1.BackgroundColor3 = Color3.fromRGB()
								healthbarOutline1.BorderColor3 = Color3.new()
								healthbarOutline1.BorderSizePixel = 0
								healthbarOutline1.Position = UDim2.fromScale(1, 0)
								healthbarOutline1.ZIndex = 2
								healthbarOutline1.Position = UDim2.new(1, 0, 0, 0)
								healthbarOutline1.Parent = horizontalHealthBar

								local uIStroke4 = Instance.new("UIStroke")
								uIStroke4.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
								uIStroke4.Parent = horizontalHealthBar

								horizontalHealthBar.Parent = top

								top.Parent = sides

								local left = Instance.new("Frame")
								left.AnchorPoint = Vector2.new(1, 0)
								left.BackgroundTransparency = 1
								left.Position = UDim2.fromOffset(-8, 0)
								left.Size = UDim2.fromScale(1, 1)

								local objects3 = Instance.new("Frame")
								objects3.AnchorPoint = Vector2.new(1, 0)
								objects3.AutomaticSize = Enum.AutomaticSize.XY
								objects3.BackgroundTransparency = 1
								objects3.Position = UDim2.fromScale(1, 0)

								local uIListLayout6 = Instance.new("UIListLayout")
								uIListLayout6.HorizontalAlignment = Enum.HorizontalAlignment.Right
								uIListLayout6.ItemLineAlignment = Enum.ItemLineAlignment.End
								uIListLayout6.Padding = UDim.new(0, 5)
								uIListLayout6.SortOrder = Enum.SortOrder.LayoutOrder
								uIListLayout6.Parent = objects3

								objects3.Parent = left

								local uIListLayout7 = Instance.new("UIListLayout")
								uIListLayout7.FillDirection = Enum.FillDirection.Horizontal
								uIListLayout7.HorizontalAlignment = Enum.HorizontalAlignment.Right
								uIListLayout7.ItemLineAlignment = Enum.ItemLineAlignment.Start
								uIListLayout7.Padding = UDim.new(0, 5)
								uIListLayout7.SortOrder = Enum.SortOrder.LayoutOrder
								uIListLayout7.Parent = left

								left.Parent = sides

								local weaponIcon = Instance.new("ImageLabel")
								weaponIcon.BackgroundTransparency = 1
								weaponIcon.Size = UDim2.new(0, 35, 0, 35)
								weaponIcon.Position = UDim2.new(0, 0, 0, 0)

								weaponIcon.Parent = right

								sides.Parent = frame
								--
								local function makelabel()
									local t = Instance.new("TextLabel")
									t.AutomaticSize = Enum.AutomaticSize.XY
									t.BackgroundTransparency = 1
									t.FontFace = textfont
									t.Text = "None"
									t.TextSize = 9
									t.TextStrokeTransparency = 0
									t.ZIndex = 3

									return t
								end

								local Class
								if plr:IsA("Player") or plr.Name:find("Rig") then
									Class = "Players"
								elseif SoldierClassType[plr.Name] then
									Class = "Soldiers"
								elseif plr:FindFirstChild("Humanoid") then
									Class = "Soldiers"
								else
									Class = "Players"
								end

								local data = {
									Frame = frame,
									Class = Class,
									Box = box,
									BoxGradient = uIGradient,
									WeaponIcon = weaponIcon,
									Corners = corners,
									CornerHolder = cornerHolder,
									Strokes = {
										Outer = uIStroke,
										Main = uIStroke1,
										Inner = uIStroke2,
									},
									Sides = {
										Top = {
											Frame = top,
											Objs = objects2,
											HB = horizontalHealthBar,
											HBFill = healthbarOutline1,
											Grad = uIGradient2,
										},
										Bottom = {
											Frame = bottom,
											Objs = objects,
											HB = horizontalHealthBar,
											HBFill = healthbarOutline1,
											Grad = uIGradient2,
										},
										Left = {
											Frame = left,
											Objs = objects3,
											HB = healthbar,
											HBFill = healthbarOutline,
											Grad = uIGradient1,
										},
										Right = {
											Frame = right,
											Objs = objects1,
											HB = healthbar,
											HBFill = healthbarOutline,
											Grad = uIGradient1,
										},
									},

									Labels = {
										Name = makelabel(),
										Distance = makelabel(),
										Weapon = makelabel(),
									},
									Chams = {},
								}

								for key, label in next, ESPFlags do
									data.Labels[key] = makelabel()
								end

								data.Frame.Parent = Holder
								Cache[plr] = data

								local function bindChar(char)
									if not char then
										return
									end
									local data = Cache[plr]
									if not data then
										return
									end

									data._connections = {}
									data.chamsholder = Instance.new("Folder")
									data.chamsholder.Parent = char

									local function makeAdornment(part)
										local inner, outer

										if part.Name == "Head" then
											inner = Instance.new("CylinderHandleAdornment")
											inner.Adornee = part
											inner.ZIndex = 2
											inner.Height = 0.87
											inner.Radius = 0.5
											inner.CFrame = CFrame.new(Vector3.new(), Vector3.new(0, 1, 0))
											inner.AlwaysOnTop = true
											inner.Parent = data.chamsholder

											outer = Instance.new("CylinderHandleAdornment")
											outer.Adornee = part
											outer.ZIndex = 2147483647
											outer.Height = 1.02
											outer.Radius = 0.65
											outer.CFrame = CFrame.new(Vector3.new(), Vector3.new(0, 1, 0))
											outer.AlwaysOnTop = true
											outer.Parent = data.chamsholder
										else
											inner = Instance.new("BoxHandleAdornment")
											inner.Adornee = part
											inner.ZIndex = 2
											inner.Size = part.Size + Vector3.new(-0.05, -0.05, -0.05)
											inner.AlwaysOnTop = true
											inner.Parent = data.chamsholder

											outer = Instance.new("BoxHandleAdornment")
											outer.Adornee = part
											outer.ZIndex = 2147483647
											outer.Size = part.Size + Vector3.new(0.05, 0.05, 0.05)
											outer.AlwaysOnTop = true
											outer.Parent = data.chamsholder
										end

										data.Chams[part] = { inner, outer }
										data._connections[part] = part:GetPropertyChangedSignal("Size"):Connect(function()
											if inner:IsA("BoxHandleAdornment") then
												inner.Size = part.Size + Vector3.new(-0.05, -0.05, -0.05)
												outer.Size = part.Size + Vector3.new(0.05, 0.05, 0.05)
											end
										end)
									end

									local function GetEquippedTool(playerName)
										local plrData = game:GetService("ReplicatedStorage").Players
											:FindFirstChild(playerName)
										if
											plrData
											and plrData:FindFirstChild("Status")
											and plrData.Status:FindFirstChild("GameplayVariables")
										then
											local toolValue =
												plrData.Status.GameplayVariables:FindFirstChild("EquippedTool")
											if toolValue and toolValue.Value ~= nil and tostring(toolValue.Value) ~= "" then
												return tostring(toolValue.Value)
											end
										end
										return "None"
									end

									for _, p in next, char:GetChildren() do
										if p:IsA("BasePart") and p.Name ~= "HumanoidRootPart" then
											makeAdornment(p)
										end
									end

									do
										local equipped = GetEquippedTool(plr.Name)
										if equipped ~= "None" then
											data.CurrentWeapon = equipped
											if data.Labels.Weapon then
												data.Labels.Weapon.Text = equipped
											end
											ItemCache[plr] = equipped
										end
									end

									data._connections["ChildAdded"] = char.ChildAdded:Connect(function(c)
										if c:IsA("BasePart") and c.Name ~= "HumanoidRootPart" then
											makeAdornment(c)
										end

										local equipped = GetEquippedTool(plr.Name)
										if equipped ~= "None" then
											data.CurrentWeapon = equipped
											if data.Labels.Weapon then
												data.Labels.Weapon.Text = equipped
											end
											ItemCache[plr] = equipped
										end
									end)

									data._connections["ChildRemoved"] = char.ChildRemoved:Connect(function(c)
										if c:IsA("BasePart") then
											local pair = data.Chams[c]
											if pair then
												if pair[1] then
													pair[1]:Destroy()
												end
												if pair[2] then
													pair[2]:Destroy()
												end
												data.Chams[c] = nil
												if data._connections[c] then
													data._connections[c]:Disconnect()
													data._connections[c] = nil
												end
											end
										elseif c == data.CurrentWeapon then
											data.CurrentWeapon = nil
											weaponIcon.Image = ""
											if data.Labels.Weapon then
												data.Labels.Weapon.Text = "None"
											end
											ItemCache[plr] = nil
										end
									end)

									if not Players:GetPlayerFromCharacter(char) then
										return
									end

									plr.CharacterRemoving:Connect(function()
										if not data then
											return
										end
										for _, pair in next, data.Chams do
											if pair[1] then
												pair[1]:Destroy()
											end
											if pair[2] then
												pair[2]:Destroy()
											end
										end
										for _, conn in next, data._connections do
											if typeof(conn) == "RBXScriptConnection" then
												conn:Disconnect()
											end
										end
										data.Chams = {}
										data._connections = {}
										data.CurrentWeapon = nil
										weaponIcon.Image = ""
										if data.Labels.Weapon then
											data.Labels.Weapon.Text = "None"
										end
										ItemCache[plr] = nil
									end)
								end

								bindChar(plr:IsA("Player") and plr.Character or plr)
								if not plr:IsA("Player") then
									return
								end
								plr.CharacterAdded:Connect(bindChar)
							end

							for _, p in next, Players:GetPlayers() do
								Espify(p)
							end
							Players.PlayerAdded:Connect(Espify)
							Players.PlayerRemoving:Connect(function(plr)
								local data = Cache[plr]
								if data then
									data.Frame:Destroy()
									Cache[plr] = nil
								end
							end)

							for i, v in pairs(game:GetService("Workspace").AiZones:GetDescendants()) do
								if v:FindFirstChild("Humanoid") then
									Espify(v)
								end
							end

							game:GetService("Workspace").AiZones.DescendantAdded:Connect(function(Child)
								wait(1)
								if Child:FindFirstChild("Humanoid") then
									Espify(Child)
								end
							end)

							local function DrawText(data, key, sideKey, text, color, tr)
								local lbl = data.Labels[key]
								if text then
									lbl.Text = text
								end
								lbl.TextColor3 = color
								lbl.TextTransparency = tr
								lbl.Parent = data.Sides[sideKey].Objs
								lbl.Visible = true
							end

							local hideESP = function(data)
								data.Frame.Visible = false
								for _, pair in next, data.Chams do
									local inner, outer = pair[1], pair[2]
									if not inner or not outer then
										continue
									end
									inner.Visible = false
									outer.Visible = false
								end
							end

							local lastUpdate = tick()

							local conn = RunService.Heartbeat:Connect(function()
								if tick() - lastUpdate < (1 / _G.espCFG.FPS) then
									return
								end
								lastUpdate = tick()
								if not _G.espCFG.Enabled then
									for _, data in next, Cache do
										hideESP(data)
									end
									return
								end

								for plr, data in next, Cache do
									local cfg = _G.espCFG[data.Class]
									local char = plr:IsA("Player") and plr.Character or plr
									local hrp = char and char:FindFirstChild("HumanoidRootPart")
									local hum = char and char:FindFirstChildOfClass("Humanoid")

									if not hrp or not hum or hum.Health <= 0 then
										hideESP(data)
										continue
									end
									if not cfg.Enabled then
										hideESP(data)
										continue
									end

									if _G.espCFG.TeamCheck and isTeam(plr) then
										hideESP(data)
										continue
									end

									local pos, size, visible, width, height = BoxMath(char)
									if not visible then
										hideESP(data)
										continue
									end

									local dist = (Camera.CFrame.Position - hrp.Position).Magnitude
									if dist > cfg.MaxDistance then
										hideESP(data)
										continue
									end

									data.Frame.Position = UDim2.fromOffset(pos.X, pos.Y)
									data.Frame.Size = UDim2.fromOffset(size.X, size.Y)

									if cfg.Box.Enabled and cfg.Box.Boxes then
										if cfg.Box.Type == "Full" then
											data.Strokes.Main.Color = cfg.Box.Color
											data.Strokes.Main.Transparency = cfg.Box.Transparency
											data.Strokes.Outer.Color = cfg.Box.OutlineColor
											data.Strokes.Outer.Transparency = cfg.Box.OutlineTransparency
											data.Strokes.Inner.Color = cfg.Box.OutlineColor
											data.Strokes.Inner.Transparency = cfg.Box.InnerOutlineTransparency
											data.Box.Visible = true
											data.CornerHolder.Visible = false
										else
											data.Box.Visible = false
											data.CornerHolder.Visible = true

											local c = data.Corners
											local thick = 1
											local minwh = math.min(width, height) / 2
											local horiz = minwh * 0.8
											local vert = horiz

											c[1][1].Size = UDim2.new(0, horiz, 0, thick)
											c[1][1].Position = UDim2.fromOffset(0, 0)

											c[2][1].Size = UDim2.new(0, thick, 0, vert)
											c[2][1].Position = UDim2.fromOffset(0, 0)

											c[3][1].Size = UDim2.new(0, horiz, 0, thick)
											c[3][1].Position = UDim2.fromOffset(width, 0)

											c[4][1].Size = UDim2.new(0, thick, 0, vert)
											c[4][1].Position = UDim2.fromOffset(width, 0)

											c[5][1].Size = UDim2.new(0, horiz, 0, thick)
											c[5][1].Position = UDim2.fromOffset(0, height)

											c[6][1].Size = UDim2.new(0, thick, 0, vert)
											c[6][1].Position = UDim2.fromOffset(0, height)

											c[7][1].Size = UDim2.new(0, horiz, 0, thick)
											c[7][1].Position = UDim2.fromOffset(width, height)

											c[8][1].Size = UDim2.new(0, thick, 0, vert)
											c[8][1].Position = UDim2.fromOffset(width, height)

											for _, corner in next, data.Corners do
												corner[1].BackgroundColor3 = cfg.Box.Color
												corner[1].BackgroundTransparency = cfg.Box.Transparency
												corner[2].BackgroundTransparency = cfg.Box.Transparency
											end
										end
									else
										data.CornerHolder.Visible = false
										data.Box.Visible = false
									end

									--if cfg.Arrows.Enabled then
									--end

									data.BoxGradient.Color = ColorSequence.new({
										ColorSequenceKeypoint.new(0, cfg.Box.FilledColor1),
										ColorSequenceKeypoint.new(1, cfg.Box.FilledColor2),
									})

									data.BoxGradient.Transparency = NumberSequence.new({
										NumberSequenceKeypoint.new(
											0,
											cfg.Box.Enabled and cfg.Box.Filled and cfg.Box.FilledTransparency1 or 1
										),
										NumberSequenceKeypoint.new(
											1,
											cfg.Box.Enabled and cfg.Box.Filled and cfg.Box.FilledTransparency2 or 1
										),
									})

									data.BoxGradient.Rotation = cfg.Box.FilledRotation

									local enabled = cfg.Chams.Enabled
									local normal = cfg.Chams.NormalColor
									local glow = cfg.Chams.GlowColor

									for _, pair in next, data.Chams do
										local inner, outer = pair[1], pair[2]
										if enabled then
											inner.Color3 = normal.Color
											inner.Transparency = normal.Transparency
											outer.Color3 = glow.Color
											outer.Transparency = glow.Transparency
											inner.Visible = true
											outer.Visible = true
										else
											inner.Visible = false
											outer.Visible = false
										end
									end

									if cfg.Name.Enabled then
										local text = (cfg.Name.UseDisplayName and plr:IsA("Player") and plr.DisplayName)
											or plr.Name
										DrawText(data, "Name", cfg.Name.Pos, text, cfg.Name.Color, cfg.Name.Transparency)
									elseif data.Labels.Name then
										data.Labels.Name.Visible = false
									end

									if cfg.Distance.Enabled then
										DrawText(
											data,
											"Distance",
											cfg.Distance.Pos,
											string.format("%dm", math.floor(dist)),
											cfg.Distance.Color,
											cfg.Distance.Transparency
										)
									elseif data.Labels.Distance then
										data.Labels.Distance.Visible = false
									end

									if cfg.Weapon.Enabled then
										DrawText(
											data,
											"Weapon",
											cfg.Weapon.Pos,
											nil,
											cfg.Weapon.Color,
											cfg.Weapon.Transparency
										)
									elseif data.Labels.Weapon then
										data.Labels.Weapon.Visible = false
									end

									if cfg.WeaponIcon.Enabled and data.WeaponIcon.Image ~= "" then
										data.WeaponIcon.Visible = true
										data.WeaponIcon.Parent = data.Sides[cfg.WeaponIcon.Pos].Objs
									else
										data.WeaponIcon.Visible = false
									end

									for _, side in next, data.Sides do
										if side.HB then
											side.HB.Visible = false
											side.HBFill.Visible = false
										end
									end

									if cfg.Health.Enabled then
										local pos = cfg.Health.Pos
										local side = data.Sides[pos]
										if side and side.HB then
											local pct = math.clamp(hum.Health / math.max(1, hum.MaxHealth), 0, 1)
											local bar = side.HB
											local HBFill = side.HBFill
											local grad = side.Grad
											local low, mid, high =
												cfg.Health.GradientLow, cfg.Health.GradientMid, cfg.Health.GradientHigh
											local thick = cfg.Health.Thickness

											if outline then
												outline.BackgroundColor3 = cfg.Health.Background
												outline.BackgroundTransparency = cfg.Health.Transparency
												outline.Visible = true
											end

											grad.Color = ColorSequence.new({
												ColorSequenceKeypoint.new(0, low),
												ColorSequenceKeypoint.new(0.5, mid),
												ColorSequenceKeypoint.new(1, high),
											})
											data.lasthealth = data.lasthealth or pct
											data.lasthealth = data.lasthealth + (pct - data.lasthealth) * 0.15
											local pct = data.lasthealth
											if pos == "Top" then
												bar.LayoutOrder = 100
												bar.Size = UDim2.new(1, 0, 0, thick)

												HBFill.AnchorPoint = Vector2.new(0, 0)
												HBFill.Position = UDim2.new(0, 0, 0, 0)
												HBFill.Size = UDim2.new(1 - pct, 0, 1, 0)
												grad.Rotation = 0
											elseif pos == "Bottom" then
												bar.LayoutOrder = -100
												bar.Size = UDim2.new(1, 0, 0, thick)

												HBFill.AnchorPoint = Vector2.new(1, 0)
												HBFill.Position = UDim2.new(1, 1, 0, 0)
												HBFill.Size = UDim2.new(1 - pct, 0, 1, 0)
												grad.Rotation = 0
											elseif pos == "Left" then
												bar.Size = UDim2.new(0, thick + 2, 1, 0)
												bar.LayoutOrder = 100
												HBFill.Size = UDim2.new(1, 0, 1 - pct, 0)
												grad.Rotation = -90
											elseif pos == "Right" then
												bar.LayoutOrder = -100
												bar.Size = UDim2.new(0, thick + 2, 1, 0)
												HBFill.Size = UDim2.new(1, 0, 1 - pct, 0)
												grad.Rotation = -90
											end

											bar.Parent = side.Frame
											bar.Visible = true
											HBFill.Visible = true
										end
									end

									if cfg.Flags.Enabled then
										for key, flagsdata in next, ESPFlags do
											if
												(flagsdata.Allow(plr) or plr.Name == "Rig")
												and cfg.Flags.EnabledFlags[key]
											then
												DrawText(
													data,
													key,
													cfg.Flags.Pos,
													flagsdata.Text(plr),
													cfg.Flags.FlagsColors[key].Color,
													cfg.Flags.FlagsColors[key].Transparency
												)
											elseif data.Labels[key] then
												data.Labels[key].Visible = false
											end
										end
									else
										for key, _ in next, ESPFlags do
											if data.Labels[key] then
												data.Labels[key].Visible = false
											end
										end
									end
									data.Frame.Visible = true
								end
							end)
						end
						-- entity
						Vis1 = {}
						function Vis1:Connection(Signal, Callback)
							local Con = Signal:Connect(Callback)
							return Con
						end
						function Vis1:CreateObject(name, tag, parent, imageId)
							local BillboardGui = Instance.new("BillboardGui")
							BillboardGui.Name = name
							BillboardGui.Adornee = parent
							BillboardGui.Size = UDim2.new(0, 30, 0, 30)
							BillboardGui.StudsOffset = Vector3.new(0, 2, 0)
							BillboardGui.AlwaysOnTop = true

							local ImageLabel = Instance.new("ImageLabel")
							ImageLabel.Name = tag
							ImageLabel.Size = UDim2.new(1, 0, 1, 0)
							ImageLabel.BackgroundTransparency = 1
							ImageLabel.Image = imageId
							ImageLabel.Parent = BillboardGui

							BillboardGui.Parent = parent
							return BillboardGui
						end
						function Vis1:CreateObject2(name, tag, parent, imageId)
							local BillboardGui = Instance.new("BillboardGui")
							BillboardGui.Name = name
							BillboardGui.Adornee = parent
							BillboardGui.Size = UDim2.new(0, 25, 0, 25)
							BillboardGui.StudsOffset = Vector3.new(0, 2, 0)
							BillboardGui.AlwaysOnTop = true
							local Frame = Instance.new("Frame")
							Frame.Size = UDim2.new(1, 0, 1, 0)
							Frame.BackgroundTransparency = 1
							Frame.Parent = BillboardGui
							local ImageLabel = Instance.new("ImageLabel")
							ImageLabel.Name = tag
							ImageLabel.Size = UDim2.new(1, 0, 0.7, 0)
							ImageLabel.Position = UDim2.new(0, 0, 0, 0)
							ImageLabel.BackgroundTransparency = 1
							ImageLabel.Image = imageId
							ImageLabel.Parent = Frame
							local TextLabel = Instance.new("TextLabel")
							TextLabel.Name = "CorpseLabel"
							TextLabel.Size = UDim2.new(1, 0, 0.3, 0)
							TextLabel.Position = UDim2.new(0, 0, 0.8, 0)
							TextLabel.BackgroundTransparency = 1
							TextLabel.Text = name
							TextLabel.TextColor3 = Color3.new(1, 1, 1)
							TextLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
							TextLabel.TextStrokeTransparency = 0
							TextLabel.TextScaled = false
							TextLabel.TextSize = 9
							TextLabel.FontFace = Visuals2.Fonts["SmallestPixel"][1]
							TextLabel.Parent = Frame

							BillboardGui.Parent = parent
							return BillboardGui
						end
						--
						do --esp
							local sections = {
								View = EspTab:Section({ Name = "Master", Size = 17 }),
								Main = EspTab:Section({ Name = "Main ESP", Size = 80, Side = "Right" }),
								Entity = EspTab:Section({ Name = "Entities", Size = 15 }),
							}

							sections.View:Toggle({
								Name = "Enabled",
								Flag = "espMaster",
								Callback = function(v)
									_G.espCFG.Enabled = v
									Flags["PlayerHolder"]:Set(v)
									_G.espCFG.Players.Enabled = v
								end,
							})
							sections.View:Slider({
								Name = "fps",
								Flag = "espfps",
								Max = 144,
								Min = 30,
								Decimals = 1,
								Suffix = " FPS",
								Callback = function(int)
									_G.espCFG.FPS = int
								end,
							})
							local s2 = sections.Main
							local p = _G.espCFG.Players
							local s = _G.espCFG.Soldiers
							--
							s2:Holder({
								Return = function(Self)
									Self:Toggle({
										Name = "Bounding Box",
										Flags = "PlayerBox",
										Callback = function(v)
											p.Box.Enabled = v
											p.Box.Boxes = v
											s.Box.Enabled = v
											s.Box.Boxes = v
											Flags["PlayerBoxHolder"]:Set(v)
										end,
									}):Picker({
										Flag = "PlayerBoxColor",
										Alpha = 0,
										Callback = function(c, t)
											p.Box.Color = c
											p.Box.Transparency = t
											s.Box.Color = c
											s.Box.Transparency = t
										end,
									})

									Self:Holder({
										Return = function(Self2)
											Self2:Dropdown({
												Flag = "PlayerBoxStyle",
												Options = { "Full", "Corners" },
												Callback = function(v)
													p.Box.Type = v
													s.Box.Type = v
												end,
											})
											local a = Self2:Toggle({
												Name = "Filled",
												Flag = "PlayerBoxFilled",
												Callback = function(v)
													p.Box.Filled = v
													s.Box.Filled = v
												end,
											})
											a:Picker({
												Flag = "PlayerBoxFill1",
												Alpha = 0,
												Callback = function(c, t)
													p.Box.FilledColor1 = c
													p.Box.FilledTransparency1 = t
													s.Box.FilledColor1 = c
													s.Box.FilledTransparency1 = t
												end,
											})
											a:Picker({
												Flag = "PlayerBoxFill2",
												Alpha = 0,
												Callback = function(c, t)
													p.Box.FilledColor2 = c
													p.Box.FilledTransparency2 = t
													s.Box.FilledColor2 = c
													s.Box.FilledTransparency2 = t
												end,
											})
										end,
										Flag = "PlayerBoxHolder",
									})

									local a = Self:Toggle({
										Name = "Chams",
										Flag = "PlayerChams",
										Callback = function(v)
											p.Chams.Enabled = v
											s.Chams.Enabled = v
										end,
									})
									a:Picker({
										Flag = "PlayerNormalClr",
										Alpha = 0.4,
										Callback = function(c, t)
											p.Chams.NormalColor.Color = c
											p.Chams.NormalColor.Transparency = t
											s.Chams.NormalColor.Color = c
											s.Chams.NormalColor.Transparency = t
										end,
									})
									a:Picker({
										Flag = "PlayerGlowClr",
										Alpha = 0.1,
										Callback = function(c, t)
											p.Chams.GlowColor.Color = c
											p.Chams.GlowColor.Transparency = t
											s.Chams.GlowColor.Color = c
											s.Chams.GlowColor.Transparency = t
										end,
									})

									local b = Self:Toggle({
										Name = "Healthbar",
										Flag = "PlayerHealthbar",
										Callback = function(v)
											p.Health.Enabled = v
											s.Health.Enabled = v
											Flags["PlayerHealthHolder"]:Set(v)
										end,
									})
									b:Picker({
										Flag = "PlayerHealthLow",
										State = Color3.fromRGB(255, 70, 70),
										Callback = function(c)
											p.Health.GradientLow = c
											s.Health.GradientLow = c
										end,
									})
									b:Picker({
										Flag = "PlayerHealthMid",
										State = Color3.fromRGB(255, 210, 90),
										Callback = function(c)
											p.Health.GradientMid = c
											s.Health.GradientMid = c
										end,
									})
									b:Picker({
										Flag = "PlayerHealthHigh",
										State = Color3.fromRGB(90, 255, 100),
										Callback = function(c)
											p.Health.GradientHigh = c
											s.Health.GradientHigh = c
										end,
									})
									Self:Holder({
										Return = function(Self2)
											Self2:Dropdown({
												Flag = "PlayerHealthStyle",
												Options = { "Top", "Bottom", "Left", "Right" },
												State = "Left",
												Callback = function(v)
													p.Health.Pos = v
													s.Health.Pos = v
												end,
											})
											Self2:Slider({
												Flag = "PlayerHealthThickness",
												Max = 5,
												Min = 1,
												Callback = function(int)
													p.Health.Thickness = int
													s.Health.Thickness = int
												end,
											})
										end,
										Flag = "PlayerHealthHolder",
									})

									Self:Toggle({
										Name = "Name",
										Flag = "PlayerName",
										Callback = function(v)
											p.Name.Enabled = v
											s.Name.Enabled = v
											Flags["PlayerNameHolder"]:Set(v)
										end,
									}):Picker({
										Flag = "PlayerNameHolder",
										Alpha = 0,
										Callback = function(c, t)
											p.Name.Color = c
											s.Name.Color = c
											p.Name.Transparency = t
											s.Name.Transparency = t
										end,
									})

									Self:Holder({
										Return = function(Self2)
											Self2:Dropdown({
												Flag = "PlayerNameStyle",
												Options = { "Top", "Bottom", "Left", "Right" },
												Callback = function(v)
													p.Name.Pos = v
													s.Name.Pos = v
												end,
											})
											Self2:Toggle({
												Name = "Use Display Name",
												Flag = "UseDisplayName",
												Callback = function(v)
													p.Name.UseDisplayName = v
												end,
											})
										end,
										Flag = "PlayerNameHolder",
									})

									Self:Toggle({
										Name = "Distance",
										Flag = "PlayerDistance",
										Callback = function(v)
											p.Distance.Enabled = v
											s.Distance.Enabled = v
											Flags["PlayerDistanceHolder"]:Set(v)
										end,
									}):Picker({
										Flag = "PlayerDistanceClr",
										Alpha = 0,
										Callback = function(c, t)
											p.Distance.Color = c
											s.Distance.Color = c
											p.Distance.Transparency = t
											s.Distance.Transparency = t
										end,
									})

									Self:Holder({
										Return = function(Self2)
											Self2:Dropdown({
												Flag = "PlayerDistanceStyle",
												Options = { "Top", "Bottom", "Left", "Right" },
												Callback = function(v)
													p.Distance.Pos = v
													s.Distance.Pos = v
												end,
											})
										end,
										Flag = "PlayerDistanceHolder",
									})

									Self:Toggle({
										Name = "Item",
										Flag = "PlayerItem",
										Callback = function(v)
											p.Weapon.Enabled = v
											Flags["PlayerItemHolder"]:Set(v)
										end,
									}):Picker({
										Flag = "PlayerItemClr",
										Alpha = 0,
										Callback = function(c, t)
											p.Weapon.Color = c
											p.Weapon.Transparency = t
										end,
									})

									Self:Holder({
										Return = function(Self2)
											Self2:Dropdown({
												Flag = "PlayerItemStyle",
												Options = { "Top", "Bottom", "Left", "Right" },
												Callback = function(v)
													p.Weapon.Pos = v
												end,
											})
										end,
										Flag = "PlayerItemHolder",
									})
									Self:Toggle({
										Name = "Include AI",
										Flag = "IncludeAi",
										Callback = function(v)
											_G.espCFG.Soldiers.Enabled = v
										end,
									})
									Self:Toggle({
										Name = "Belt Viewer",
										Flag = "BeltViewer",
										Callback = function(State)
											Belt.Objects["Outline"].Visible = State
											Belt.Objects["Frame"].Visible = State
										end,
									})

									Self:Toggle({
										Name = "Armor Viewer",
										Flag = "ArmorViewer",
										Callback = function(State)
											Armor.Objects["Outline"].Visible = State
											Armor.Objects["Frame"].Visible = State
										end,
									})
								end,
								Flag = "PlayerHolder",
							})
							local createdVisuals = {}
							local childAddedConn = nil
							sections.Entity:Toggle({
								Name = "Extract",
								Flag = "ExtractEsp",
								Callback = function(v)
									if v then
										local NoCollision = workspace:FindFirstChild("NoCollision")
										if NoCollision then
											local ExitLocations = NoCollision:FindFirstChild("ExitLocations")
											if ExitLocations then
												for _, Exit in pairs(ExitLocations:GetChildren()) do
													local obj = Vis1:CreateObject(
														"Exit Location",
														"Exit",
														Exit,
														"http://www.roblox.com/asset/?id=14239902836"
													)
													table.insert(createdVisuals, obj)
												end
												childAddedConn = ExitLocations.ChildAdded:Connect(function(Exit)
													local obj = Vis1:CreateObject(
														"Exit Location",
														"Exit",
														Exit,
														"http://www.roblox.com/asset/?id=14239902836"
													)
													table.insert(createdVisuals, obj)
												end)
											end
										end
									else
										for _, obj in ipairs(createdVisuals) do
											local success, err = pcall(function()
												if obj and typeof(obj.Destroy) == "function" then
													obj:Destroy()
												end
											end)
											if not success then
												warn("Failed to destroy object:", err)
											end
										end
										table.clear(createdVisuals)
										if childAddedConn then
											childAddedConn:Disconnect()
											childAddedConn = nil
										end
									end
								end,
							})
							--sections.Entity
							--	:Toggle({
							--		Name = "Airdrops",
							--		Flag = "AirdropEsp",
							--		Callback = function(v) end,
							--	})
							--	:Picker({
							--		Flag = "AirdropClr",
							--		Alpha = 0,
							--		Callback = function(c, t) end,
							--	})
							local createdVisuals2 = {}
							local childAddedConn2 = nil
							sections.Entity:Toggle({
								Name = "Corpse",
								Flag = "CorpseEsp",
								Callback = function(v)
									if v then
										local DroppedItems = workspace:FindFirstChild("DroppedItems")
										if DroppedItems then
											childAddedConn1 = DroppedItems.ChildAdded:Connect(function(Item)
												task.wait(1)

												local playerMatch = Services.Players:FindFirstChild(Item.Name)
												local head = Item:FindFirstChild("Head")

												if playerMatch and head then
													local obj = Vis1:CreateObject2(
														"Corpse of " .. Item.Name,
														"Corpse",
														Item.PrimaryPart,
														"http://www.roblox.com/asset/?id=14239904666"
													)
													table.insert(createdVisuals2, obj)
												end
											end)

											childAddedConn2 = DroppedItems.ChildAdded:Connect(function(Exit)
												local head = Exit:FindFirstChild("Head")
												if head then
													local obj = Vis1:CreateObject2(
														"Corpse of " .. Exit.Name,
														"Corpse",
														Exit.PrimaryPart,
														"http://www.roblox.com/asset/?id=14239904666"
													)
													table.insert(createdVisuals2, obj)
												end
											end)
										end
									else
										for _, obj in ipairs(createdVisuals2) do
											local success, err = pcall(function()
												if obj and typeof(obj.Destroy) == "function" then
													obj:Destroy()
												end
											end)
											if not success then
												warn("Failed to destroy object:", err)
											end
										end
										table.clear(createdVisuals2)
										if childAddedConn2 then
											childAddedConn2:Disconnect()
											childAddedConn2 = nil
										end
									end
								end,
							})
						end
					end
					do -- skins tab
						local SkinsTab = Window:Page({ Name = "Skins" })
						do -- sections
							Custom, ToggleSection, DropdownSection =
								SkinsTab:Section({ Name = "Viewmodel" }),
								SkinsTab:Section({ Name = "Toggles", Side = "Right", Size = 50 }),
								SkinsTab:Section({ Name = "Skins", Side = "Right", Size = 50 })
						end
						do
							--
							do -- custom
								Custom:Toggle({
									Name = "Weapon Shaders",
									Flag = "ShadersToggle",
									Callback = function(v)
										Flags["ShaderHolder"]:Set(v)
										Cheat.Options.Misc.Custom.ShaderEnabled = v
									end,
								})

								Custom:Holder({
									Return = function(Self)
										Self:Label({ Name = "Scaling", Inlayed = true })
										Self:Slider({
											Flag = "TileAmount",
											Max = 10,
											Min = 1,
											Decimals = 0.1,
											Callback = function(v)
												Cheat.Options.Misc.Custom.ShaderTile = v
											end,
										})
										Self:Label({ Name = "Speed", Inlayed = true })
										Self:Slider({
											Flag = "ShaderSpeed",
											Max = 10,
											Min = 0.01,
											Decimals = 0.01,
											Callback = function(v)
												Cheat.Options.Misc.Custom.ShaderSpeed = v
											end,
										})
										Self:Dropdown({
											Flag = "ShaderOptions",
											Max = 1,
											Options = {
												"Galaxy",
												"Universe",
												"Groovy",
												"Liquid",
												"Israel",
												"Troll",
												"Russia",
												"Belarus",
												"Zelensky",
											},
											Callback = function(v)
												Cheat.Options.Misc.Custom.Shader = v
											end,
										})
									end,
									Flag = "ShaderHolder",
								})
								--Custom:Toggle({
								--	Name = "Customize",
								--	Flag = "CustomizeSkinToggle",
								--	Tooltip = "Skin creator is in beta",
								--	Callback = function(v)
								--		-- skinMaker:Switch(v)
								--	end,
								--})
								Custom:Toggle({
									Name = "Item Chams",
									Flag = "ItemChams",
									Callback = function(v)
										Flags["ItemChamsHolder"]:Set(v)
									end,
								}):Picker({
									Flag = "ItemChamsPicker",
									Alpha = 1,
								})
								Custom:Holder({
									Return = function(Self)
										Self:Dropdown({
											Flag = "ItemChamsMaterial",
											Options = {},
											Callback = function(v) end,
										})
									end,
									Flag = "ItemChamsHolder",
								})
							end
							do -- toggles
								local Knives = {}
								do
									Knives.AnarchyTomahawk = {
										weaponOffSet = CFrame.new(-0.15, -1.51, 0.2),
										sprintOffSet = Vector3.new(0, 0, -0.4),
										AimInSpeed = 0.4,
										AimOutSpeed = 0.4,
										swayMult = 1,
										useDof = true,
										allowAiming = false,
										useModuleName = "MeleeWeaponDefault",
										WeldHand = "UpperTorso",
										FireMode = "Melee",
										Scope = nil,
										EquipTValue = 0,
										AimWhileActing = true,
										MaximumKickBack = 1,
										MaxRecoil = 4,
										ReductionStartTime = 15,
										RecoilReductionMax = 1,
										RecoilTValueMax = 5,
										IdleSwayModifier = 8,
										WalkSwayModifer = 1,
										SprintSwayModifer = 1,
										ItemLength = 3.1,
										TouchWallPosY = -2.8,
										TouchWallPosZ = 2,
										TouchWallRotX = 40,
										TouchWallRotY = -9,
										FireModes = { "Melee" },
										Animations = {
											FirstPerson = {
												Equip = "rbxassetid://10905772228",
												Idle = "rbxassetid://10905775438",
												Use = "rbxassetid://10905779171",
												UseAlt = "rbxassetid://10905777783",
												Stab = "rbxassetid://10905780539",
												Inspect = "rbxassetid://10905776735",
											},
											ThirdPerson = {
												Equip = "rbxassetid://10989619410",
												Idle = "rbxassetid://10989624543",
												Use = "rbxassetid://10989646536",
												UseAlt = "rbxassetid://10989640707",
												Stab = "rbxassetid://10989574087",
												Inspect = "rbxassetid://10989633266",
											},
										},
									}
									Knives.DV2 = {
										weaponOffSet = CFrame.new(0.05, -1.3, 1),
										sprintOffSet = Vector3.new(0, 0, 1),
										AimInSpeed = 0.4,
										AimOutSpeed = 0.4,
										swayMult = 1,
										useDof = true,
										allowAiming = false,
										useModuleName = "MeleeWeaponDefault",
										WeldHand = "UpperTorso",
										FireMode = "Melee",
										Scope = nil,
										EquipTValue = 0,
										AimWhileActing = true,
										MaximumKickBack = 1,
										MaxRecoil = 4,
										ReductionStartTime = 15,
										RecoilReductionMax = 1,
										RecoilTValueMax = 5,
										IdleSwayModifier = 8,
										WalkSwayModifer = 1,
										SprintSwayModifer = 1,
										ItemLength = 3.1,
										TouchWallPosY = -2.8,
										TouchWallPosZ = 2,
										TouchWallRotX = 40,
										TouchWallRotY = -9,
										FireModes = { "Melee" },
										Animations = {
											FirstPerson = {
												Equip = "rbxassetid://8982799578",
												Idle = "rbxassetid://7963518001",
												Use = "rbxassetid://7963516952",
												UseAlt = "rbxassetid://7963600020",
												Stab = "rbxassetid://7963980103",
												Inspect = "rbxassetid://7963613236",
											},
											ThirdPerson = {
												Equip = "rbxassetid://8384325184",
												Idle = "rbxassetid://8219701028",
												Use = "rbxassetid://8219689820",
												UseAlt = "rbxassetid://8219699063",
												Stab = "rbxassetid://8219745355",
												Inspect = "rbxassetid://8219716861",
											},
										},
									}
									Knives.GoldenDV2 = {
										weaponOffSet = CFrame.new(0.05, -1.3, 1),
										sprintOffSet = Vector3.new(0, 0, 1),
										AimInSpeed = 0.4,
										AimOutSpeed = 0.4,
										swayMult = 1,
										useDof = true,
										allowAiming = false,
										useModuleName = "MeleeWeaponDefault",
										WeldHand = "UpperTorso",
										FireMode = "Melee",
										Scope = nil,
										EquipTValue = 0,
										AimWhileActing = true,
										MaximumKickBack = 1,
										MaxRecoil = 4,
										ReductionStartTime = 15,
										RecoilReductionMax = 1,
										RecoilTValueMax = 5,
										IdleSwayModifier = 8,
										WalkSwayModifer = 1,
										SprintSwayModifer = 1,
										ItemLength = 3.1,
										TouchWallPosY = -2.8,
										TouchWallPosZ = 2,
										TouchWallRotX = 40,
										TouchWallRotY = -9,
										FireModes = { "Melee" },
										Animations = {
											FirstPerson = {
												Equip = "rbxassetid://8982799578",
												Idle = "rbxassetid://8219701028",
												Use = "rbxassetid://7963516952",
												UseAlt = "rbxassetid://7963600020",
												Stab = "rbxassetid://7963980103",
												Inspect = "rbxassetid://7963613236",
											},
											ThirdPerson = {
												Equip = "rbxassetid://8384325184",
												Idle = "rbxassetid://8219701028",
												Use = "rbxassetid://8219689820",
												UseAlt = "rbxassetid://8219699063",
												Stab = "rbxassetid://8219745355",
												Inspect = "rbxassetid://8219716861",
											},
										},
									}
									Knives.M9Fade = {
										weaponOffSet = CFrame.new(0.05, -1.3, 1),
										sprintOffSet = Vector3.new(0, 0, 1),
										AimInSpeed = 0.4,
										AimOutSpeed = 0.4,
										swayMult = 1,
										useDof = true,
										allowAiming = false,
										useModuleName = "MeleeWeaponDefault",
										WeldHand = "UpperTorso",
										FireMode = "Melee",
										Scope = nil,
										EquipTValue = 0,
										AimWhileActing = true,
										MaximumKickBack = 1,
										MaxRecoil = 4,
										ReductionStartTime = 15,
										RecoilReductionMax = 1,
										RecoilTValueMax = 5,
										IdleSwayModifier = 8,
										WalkSwayModifer = 1,
										SprintSwayModifer = 1,
										ItemLength = 3.1,
										TouchWallPosY = -2.8,
										TouchWallPosZ = 2,
										TouchWallRotX = 40,
										TouchWallRotY = -9,
										FireModes = { "Melee" },
										Animations = {
											FirstPerson = {
												Equip = "rbxassetid://8982799578",
												Idle = "rbxassetid://7963518001",
												Use = "rbxassetid://7963516952",
												UseAlt = "rbxassetid://7963600020",
												Stab = "rbxassetid://7963980103",
												Inspect = "rbxassetid://7963613236",
											},
											ThirdPerson = {
												Equip = "rbxassetid://8384325184",
												Idle = "rbxassetid://8219701028",
												Use = "rbxassetid://8219689820",
												UseAlt = "rbxassetid://8219699063",
												Stab = "rbxassetid://8219745355",
												Inspect = "rbxassetid://8219716861",
											},
										},
									}
									Knives.IceAxe = {
										weaponOffSet = CFrame.new(-0.15, -1.51, 0.2),
										sprintOffSet = Vector3.new(0, 0, -0.4),
										AimInSpeed = 0.4,
										AimOutSpeed = 0.4,
										swayMult = 1,
										useDof = true,
										allowAiming = false,
										useModuleName = "MeleeWeaponDefault",
										WeldHand = "UpperTorso",
										FireMode = "Melee",
										Scope = nil,
										EquipTValue = 0,
										AimWhileActing = true,
										MaximumKickBack = 1,
										MaxRecoil = 4,
										ReductionStartTime = 15,
										RecoilReductionMax = 1,
										RecoilTValueMax = 5,
										IdleSwayModifier = 8,
										WalkSwayModifer = 1,
										SprintSwayModifer = 1,
										ItemLength = 3.1,
										TouchWallPosY = -2.8,
										TouchWallPosZ = 2,
										TouchWallRotX = 40,
										TouchWallRotY = -9,
										FireModes = { "Melee" },
										Animations = {
											FirstPerson = {
												Equip = "rbxassetid://10905772228",
												Idle = "rbxassetid://10905775438",
												Use = "rbxassetid://10905779171",
												UseAlt = "rbxassetid://10905777783",
												Stab = "rbxassetid://10905780539",
												Inspect = "rbxassetid://10905776735",
											},
											ThirdPerson = {
												Equip = "rbxassetid://10989619410",
												Idle = "rbxassetid://10989624543",
												Use = "rbxassetid://10989646536",
												UseAlt = "rbxassetid://10989640707",
												Stab = "rbxassetid://10989574087",
												Inspect = "rbxassetid://10989633266",
											},
										},
									}
									Knives.Karambit = {
										weaponOffSet = CFrame.new(0.05, -1.3, 1),
										sprintOffSet = Vector3.new(0, 0, 1),
										AimInSpeed = 0.4,
										AimOutSpeed = 0.4,
										swayMult = 1,
										useDof = true,
										allowAiming = false,
										useModuleName = "MeleeWeaponDefault",
										WeldHand = "UpperTorso",
										FireMode = "Melee",
										Scope = nil,
										EquipTValue = 0,
										AimWhileActing = true,
										MaximumKickBack = 1,
										MaxRecoil = 4,
										ReductionStartTime = 15,
										RecoilReductionMax = 1,
										RecoilTValueMax = 5,
										IdleSwayModifier = 8,
										WalkSwayModifer = 1,
										SprintSwayModifer = 1,
										ItemLength = 3.1,
										TouchWallPosY = -2.8,
										TouchWallPosZ = 2,
										TouchWallRotX = 40,
										TouchWallRotY = -9,
										FireModes = { "Melee" },
										Animations = {
											FirstPerson = {
												Equip = "rbxassetid://13660199419",
												Idle = "rbxassetid://13660201544",
												Use = "rbxassetid://13660212475",
												UseAlt = "rbxassetid://13660218031",
												Stab = "rbxassetid://13660208464",
												Inspect = "rbxassetid://13660189579",
											},
											ThirdPerson = {
												Equip = "rbxassetid://8384325184",
												Idle = "rbxassetid://8219701028",
												Use = "rbxassetid://8219689820",
												UseAlt = "rbxassetid://8219699063",
												Stab = "rbxassetid://8219745355",
												Inspect = "rbxassetid://8219716861",
											},
										},
									}
									Knives.PlasmaNinjato = {
										weaponOffSet = CFrame.new(0.1, -1.4, 0.1),
										sprintOffSet = Vector3.new(1, -1, 1),
										AimInSpeed = 0.4,
										AimOutSpeed = 0.4,
										swayMult = 1,
										useDof = true,
										allowAiming = false,
										useModuleName = "MeleeWeaponDefault",
										WeldHand = "UpperTorso",
										FireMode = "Melee",
										Scope = nil,
										EquipTValue = 0,
										AimWhileActing = true,
										MaximumKickBack = 1,
										MaxRecoil = 4,
										ReductionStartTime = 15,
										RecoilReductionMax = 1,
										RecoilTValueMax = 5,
										IdleSwayModifier = 8,
										WalkSwayModifer = 1,
										SprintSwayModifer = 1,
										ItemLength = 3.1,
										TouchWallPosY = -2.8,
										TouchWallPosZ = 2,
										TouchWallRotX = 40,
										TouchWallRotY = -9,
										FireModes = { "Melee" },
										Animations = {
											FirstPerson = {
												Equip = "rbxassetid://9602813265",
												Idle = "rbxassetid://9602814995",
												Use = "rbxassetid://9602882117",
												UseAlt = "rbxassetid://9602837643",
												Stab = "rbxassetid://9602895599",
												Inspect = "rbxassetid://9602906201",
											},
											ThirdPerson = {
												Equip = "rbxassetid://11305769153",
												Idle = "rbxassetid://11305800944",
												Sprint = "rbxassetid://11305866272",
												Use = "rbxassetid://11305837531",
												UseAlt = "rbxassetid://11305821864",
												Stab = "rbxassetid://11305793672",
												Inspect = "rbxassetid://8219716861",
											},
										},
									}
									Knives.IceDagger = {
										weaponOffSet = CFrame.new(0.05, -1.3, 1),
										sprintOffSet = Vector3.new(0, 0, 1),
										AimInSpeed = 0.4,
										AimOutSpeed = 0.4,
										swayMult = 1,
										useDof = true,
										allowAiming = false,
										useModuleName = "MeleeWeaponDefault",
										WeldHand = "UpperTorso",
										FireMode = "Melee",
										Scope = nil,
										EquipTValue = 0,
										AimWhileActing = true,
										MaximumKickBack = 1,
										MaxRecoil = 4,
										ReductionStartTime = 15,
										RecoilReductionMax = 1,
										RecoilTValueMax = 5,
										IdleSwayModifier = 8,
										WalkSwayModifer = 1,
										SprintSwayModifer = 1,
										ItemLength = 3.1,
										TouchWallPosY = -2.8,
										TouchWallPosZ = 2,
										TouchWallRotX = 40,
										TouchWallRotY = -9,
										FireModes = { "Melee" },
										Animations = {
											FirstPerson = {
												Equip = "rbxassetid://8982799578",
												Idle = "rbxassetid://7963518001",
												Use = "rbxassetid://7963516952",
												UseAlt = "rbxassetid://7963600020",
												Stab = "rbxassetid://7963980103",
												Inspect = "rbxassetid://7963613236",
											},
											ThirdPerson = {
												Equip = "rbxassetid://8384325184",
												Idle = "rbxassetid://8219701028",
												Use = "rbxassetid://8219689820",
												UseAlt = "rbxassetid://8219699063",
												Stab = "rbxassetid://8219745355",
												Inspect = "rbxassetid://8219716861",
											},
										},
									}
									Knives.Karambit = {
										weaponOffSet = CFrame.new(1.1, -1.3, 0.4),
										sprintOffSet = Vector3.new(0, 0, 1),
										AimInSpeed = 0.4,
										AimOutSpeed = 0.4,
										swayMult = 1,
										useDof = true,
										allowAiming = false,
										useModuleName = "MeleeWeaponDefault",
										WeldHand = "UpperTorso",
										FireMode = "Melee",
										Scope = nil,
										EquipTValue = 0,
										AimWhileActing = true,
										MaximumKickBack = 1,
										MaxRecoil = 4,
										ReductionStartTime = 15,
										RecoilReductionMax = 1,
										RecoilTValueMax = 5,
										IdleSwayModifier = 8,
										WalkSwayModifer = 1,
										SprintSwayModifer = 1,
										ItemLength = 3.1,
										TouchWallPosY = -2.8,
										TouchWallPosZ = 2,
										TouchWallRotX = 40,
										TouchWallRotY = -9,
										FireModes = { "Melee" },
										Animations = {
											FirstPerson = {
												Equip = "rbxassetid://13660199419",
												Idle = "rbxassetid://13660201544",
												Use = "rbxassetid://13660212475",
												UseAlt = "rbxassetid://13660218031",
												Stab = "rbxassetid://13660208464",
												Inspect = "rbxassetid://13660189579",
											},
											ThirdPerson = {
												Equip = "rbxassetid://8384325184",
												Idle = "rbxassetid://8219701028",
												Use = "rbxassetid://8219689820",
												UseAlt = "rbxassetid://8219699063",
												Stab = "rbxassetid://8219745355",
												Inspect = "rbxassetid://8219716861",
											},
										},
									}
									Knives.Cutlass = {
										["TouchWallRotY"] = -9,
										["Animations"] = {
											["ThirdPerson"] = {
												["Idle"] = "rbxassetid://8219701028",
												["Stab"] = "rbxassetid://14376493027",
												["Equip"] = "rbxassetid://14376499349",
												["Inspect"] = "rbxassetid://8219716861",
												["UseAlt"] = "rbxassetid://8219699063",
												["Use"] = "rbxassetid://8219689820",
											},
											["FirstPerson"] = {
												["Idle"] = "rbxassetid://7963518001",
												["Stab"] = "rbxassetid://14326935050",
												["Equip"] = "rbxassetid://14326929299",
												["Inspect"] = "rbxassetid://7963613236",
												["UseAlt"] = "rbxassetid://7963600020",
												["Use"] = "rbxassetid://7963516952",
											},
										},
										["WeldHand"] = "UpperTorso",
										["MaximumKickBack"] = 1,
										["ReductionStartTime"] = 15,
										["TouchWallPosY"] = -2.8,
										["AimWhileActing"] = true,
										["TouchWallRotX"] = 40,
										["FireModes"] = { [1] = "Melee" },
										["AimOutSpeed"] = 0.4,
										["ItemLength"] = 3.1,
										["WalkSwayModifer"] = 1,
										["useDof"] = true,
										["AimInSpeed"] = 0.4,
										["EquipTValue"] = 0,
										["IdleSwayModifier"] = 8,
										["FireMode"] = "Melee",
										["allowAiming"] = false,
										["MaxRecoil"] = 4,
										["TouchWallPosZ"] = 2,
										["useModuleName"] = "MeleeWeaponDefault",
										["RecoilReductionMax"] = 1,
										sprintOffSet = Vector3.new(0, 0, 1),
										weaponOffSet = CFrame.new(0.0500000007, -1.29999995, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1),
										--['weaponOffSet'] = '0.0500000007, -1.29999995, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1',
										["swayMult"] = 1,
										["RecoilTValueMax"] = 5,
										["SprintSwayModifer"] = 1,
									}
								end
								--
								local Plr = game:GetService("Players").LocalPlayer
								--
								SkinnableWeapons = {}
								local SkinTypes = {}
								--

								for Index, Weapon in next, game:GetService("ReplicatedStorage").RangedWeapons:GetChildren() do
									if
										game:GetService("ReplicatedStorage").Skins:FindFirstChild(Weapon.Name)
										and #game:GetService("ReplicatedStorage").Skins[Weapon.Name]:GetChildren() > 1
									then
										table.insert(SkinnableWeapons, Weapon.Name)
										SkinTypes[Weapon.Name] = {}
										for _, Skin in
											next,
											game:GetService("ReplicatedStorage").Skins[Weapon.Name]:GetChildren()
										do
											table.insert(SkinTypes[Weapon.Name], Skin.Name)
										end
									end
								end
								local skinStates = {}
								local knifeState = {
									enabled = false,
									selectedKnife = "DV2",
								}

								local function applyskins(Plr, skinStates, knifeState, Knives)
									local ReplicatedStorage = game:GetService("ReplicatedStorage")
									local playerFolder = ReplicatedStorage:WaitForChild("Players"):FindFirstChild(Plr.Name)
									if not playerFolder then
										return
									end

									for weaponName, state in pairs(skinStates) do
										if state.enabled then
											for _, value in next, playerFolder:GetDescendants() do
												local canSkin = false
												if value.Name == weaponName or value:FindFirstAncestor(weaponName) then
													for name, component in pairs(value:GetAttributes()) do
														local compStr = tostring(component)
														if
															(name == "CallSign" and compStr:find(weaponName))
															or compStr:find("vz")
															or compStr:find("PM")
															or compStr:find("Saiga")
															or compStr:find("Val")
															or compStr:find("Stock")
															or compStr:find("Front")
														then
															canSkin = true
															break
														end
													end
													if canSkin then
														value:SetAttribute("Skin", state.selectedSkin)
													end
												end
											end
										end
									end

									if knifeState.enabled then
										local knifeNames = {
											"IceDagger",
											"GoldenDV2",
											"M9Fade",
											"IceAxe",
											"Karambit",
											"PlasmaNinjato",
											"DV2",
											"AnarchyTomahawk",
										}
										for _, v in next, playerFolder:GetDescendants() do
											if table.find(knifeNames, v.Name) then
												v.Name = knifeState.selectedKnife
												local settingsModule = v:FindFirstChild("SettingsModule")
												if settingsModule then
													local script = require(settingsModule)
													for i in next, script do
														if
															Knives[knifeState.selectedKnife]
															and Knives[knifeState.selectedKnife][i]
														then
															script[i] = Knives[knifeState.selectedKnife][i]
														end
													end
												end
											end
										end
									end
								end

								ToggleSection:Toggle({
									Name = "Knife",
									Flag = "ofov",
									Callback = function(state)
										knifeState.enabled = state
										applyskins(Plr, skinStates, knifeState, Knives)
									end,
								})

								DropdownSection:Dropdown({
									Flag = "knifeskins",
									Options = {
										"DV2",
										"GoldenDV2",
										"M9Fade",
										"IceAxe",
										"IceDagger",
										"AnarchyTomahawk",
										"PlasmaNinjato",
									},
									State = 1,
									Callback = function(State)
										knifeState.selectedKnife = State
										applyskins(Plr, skinStates, knifeState, Knives)
									end,
								})

								for _, weaponName in ipairs(SkinnableWeapons) do
									skinStates[weaponName] = {
										enabled = false,
										selectedSkin = SkinTypes[weaponName][1] or "Default",
									}
									--
									ToggleSection:Toggle({
										Name = weaponName,
										Flag = weaponName .. "Enabled",
										Callback = function(state)
											skinStates[weaponName].enabled = state
											applyskins(Plr, skinStates, knifeState, Knives)
										end,
									})
									--
									DropdownSection:Dropdown({
										Options = SkinTypes[weaponName],
										Flag = weaponName .. "Skins",
										State = "Default",
										Callback = function(state)
											skinStates[weaponName].selectedSkin = state
											applyskins(Plr, skinStates, knifeState, Knives)
										end,
									})
								end
							end
						end
						--
						do -- misc tab
							local MiscTab = Window:Page({ Name = "Misc" })
							--
							do -- sections
								miscChar = MiscTab:Section({ Name = "Character", Side = "Left", Size = 40 })
								miscSync = MiscTab:Section({ Name = "Replication", Side = "Left", Size = 60 })
							end
							--
							do -- toggles
								-- character
								miscChar:Label({ Name = "Character Modifications" })
								miscChar:Dropdown({ -- normalize output if string or if table xd
									Flag = "charmods",
									Options = {
										"No BrokenBones",
										"No Bleeding",
										"No Fall Damage",
										"No Weight",
										"No Radiation",
										"Infinite Food",
										"Infinite Water",
										"No Drown",
										"No Jump Cooldown",
									},
									State = "",
									Max = 999,
									Min = 0,
									Callback = function(v) end,
								})
								miscChar:Button({
									Name = "Suicide",
									Flag = "Suicide",
									Callback = function(v) end,
								})
								miscChar
									:Toggle({
										Name = "Movement Bypass",
										Flag = "MovementBypass",
									})
									:Bind({
										Name = "Movement Bypass",
										Flag = "MovementBypassBind",
									})
								miscChar:Toggle({
									Name = "Speedhack",
									Flag = "Speedhack",
									Callback = function(State)
										Flags["Speedhack Holder"]:Set(State)
									end,
								})
								miscChar:Holder({
									Return = function(Self)
										--
										Self:Label({ Name = "Speed", Inlayed = true })
										Self:Slider({
											Flag = "Speedhack Value",
											Max = 350,
											Min = 1,
											Default = 16,
											Suffix = "studs/s",
										})
									end,
									--
									Flag = "Speedhack Holder",
								})
								--
								miscChar:Toggle({
									Name = "Jumphack",
									Flag = "Jumphack",
									Callback = function(State)
										Flags["Jumphack Holder"]:Set(State)
									end,
								})
								--
								miscChar:Holder({
									Return = function(Self)
										Self:Label({ Name = "Power", Inlayed = true })
										Self:Slider({
											Flag = "Jumphack Value",
											Max = 350,
											Min = 1,
											Decimals = 0.1,
											Default = 16,
										})
									end,
									--
									Flag = "Jumphack Holder",
								})
								-- desync
								miscSync
									:Toggle({
										Name = "Replication",
										Flag = "Replication",
										Callback = function(State) end,
									})
									:Bind({
										Flag = "Key",
										Mode = "On Toggle",
									})
								--
								miscSync:Toggle({
									Name = "Underground",
									Flag = "Replication Always Stay Underground",
								})

								--
								local Chams = miscSync:Toggle({
									Name = "Visualization",
									Flag = "Visualization",
									Callback = function(State)
										Flags["Visualization Holder"]:Set(State)
									end,
								})
								Chams:Picker({
									State = Color3.fromRGB(0, 255, 234),
									Flag = "Fill Color",
									Alpha = 0.5,
								})
								Chams:Picker({
									State = Color3.fromRGB(136, 16, 206),
									Flag = "Outline Color",
									Alpha = 0.5,
								})
								miscSync:Holder({
									Return = function(Self)
										--
										Self:Label({ Name = "Material", Inlayed = true })
										Self:Dropdown({
											Flag = "Visualization Material",
											Options = { "SmoothPlastic", "ForceField", "Neon" },
											State = 1,
										})
									end,
									--
									Flag = "Visualization Holder",
								})
								--
								ls = { enabled = false, bind = false, value = 60 }

								miscSync
									:Toggle({
										Name = "Lag Switch",
										Flag = "Lag Switch",
										Callback = function(State)
											Flags["Lag Switch Holder"]:Set(State)
											ls.enabled = State
										end,
									})
									:Bind({
										Flag = "Key",
										Mode = "On Hold",
										Callback = function(state)
											ls.bind = state
										end,
									})

								miscSync:Holder({
									Return = function(Self)
										Self:Label({ Name = "Ticks", Inlayed = true })
										Self:Slider({
											Flag = "Lag Switch Ticks",
											Max = 12500,
											Min = 1,
											Default = 60,
											Suffix = "ms",
											Callback = function(state)
												ls.value = state
											end,
										})
									end,
									Flag = "Lag Switch Holder",
								})

								-- // Lag Switch Handler
								task.spawn(function()
									while task.wait() do
										if ls.enabled and ls.bind then
											settings():GetService("NetworkSettings").IncomingReplicationLag = ls.value
												/ 1000
										else
											settings():GetService("NetworkSettings").IncomingReplicationLag = 0
										end
									end
								end)

								miscSync:Label({ Name = "Replication Mode", Inlayed = true })
								miscSync:Dropdown({
									Flag = "Replication Modes",
									Options = { "Custom", "Random", "None" },
									State = "None",
									Maximum = 1,
									Minimum = 1,
									Callback = function(v)
										Flags["CustomMode"]:Set(v == "Custom")
										Flags["RandomMode"]:Set(v == "Random")
									end,
								})
								--
								miscSync:Holder({
									Return = function(Self)
										Self:Label({ Name = "Position X", Inlayed = true })
										Self:Slider({
											Flag = "posx",
											Max = 3,
											Min = -3,
											Decimals = 0.1,
											Default = 0,
											Suffix = "st",
										})
										--
										Self:Label({ Name = "Position Y", Inlayed = true })
										Self:Slider({
											Flag = "posy",
											Max = 3,
											Min = -3,
											Decimals = 0.1,
											Default = 0,
											Suffix = "st",
										})
										--
										Self:Label({ Name = "Position Z", Inlayed = true })
										Self:Slider({
											Flag = "posz",
											Max = 3,
											Min = -3,
											Decimals = 0.1,
											Default = 0,
											Suffix = "st",
										})
										-- rotation
										Self:Label({ Name = "Rotation X", Inlayed = true })
										Self:Slider({
											Flag = "rotx",
											Max = 180,
											Min = -180,
											Decimals = 1,
											Default = 0,
											Suffix = "°",
										})
										--
										Self:Label({ Name = "Rotation Y", Inlayed = true })
										Self:Slider({
											Flag = "roty",
											Max = 180,
											Min = -180,
											Decimals = 1,
											Default = 0,
											Suffix = "°",
										})
										--
										Self:Label({ Name = "Rotation Z", Inlayed = true })
										Self:Slider({
											Flag = "rotZ",
											Max = 180,
											Min = -180,
											Decimals = 1,
											Default = 0,
											Suffix = "°",
										})
									end,
									--
									Flag = "CustomMode",
								})

								--

								miscSync:Holder({
									Return = function(Self)
										Self:Label({ Name = "Range", Inlayed = true })
										Self:Slider({
											Flag = "randomrange",
											Max = 3,
											Min = 0.1,
											Decimals = 0.1,
											Default = 2,
											Suffix = "st",
										})
									end,
									--
									Flag = "RandomMode",
								})
							end
						end
						--
						do -- initialize
							Window:SettingsPage()
							Window.Pages[1]:Set(true)
							Window:Initialise()
						end
					end
				end
			end
		end)()

		FovOutline = CreateInstance("Frame", {
			Name = "FovOutline",
			Parent = gethui(),
			BackgroundColor3 = Color3.fromRGB(255, 255, 255),
			BackgroundTransparency = 1.000,
			AnchorPoint = Vector2.new(0.5, 0.5),
			BorderColor3 = Color3.fromRGB(0, 0, 0),
			BorderSizePixel = 0,
			Position = UDim2.new(0.5, -Cheat.Options.SilentAim.Fov.Size, 0.5, -Cheat.Options.SilentAim.Fov.Size),
			Size = UDim2.new(0, Cheat.Options.SilentAim.Fov.Size * 2, 0, Cheat.Options.SilentAim.Fov.Size * 2),
		})

		OutlineRounding = CreateInstance("UICorner", {
			CornerRadius = UDim.new(1, 0),
			Name = "OutlineRounding",
			Parent = FovOutline,
		})

		Outline = CreateInstance("UIStroke", {
			Parent = FovOutline,
			Enabled = true,
			Transparency = 0,
			Color = Color3.fromRGB(0, 0, 0),
			ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual,
			LineJoinMode = Enum.LineJoinMode.Round,
			Thickness = 4,
		})

		FovInline = CreateInstance("Frame", {
			Parent = FovOutline,
			BackgroundColor3 = Color3.fromRGB(255, 255, 255),
			BackgroundTransparency = 1.000,
			BorderColor3 = Color3.fromRGB(0, 0, 0),
			BorderSizePixel = 0,
			Position = UDim2.new(0, -1, 0, -1),
			Size = UDim2.new(1, 2, 1, 2),
		})

		InlineRounding = CreateInstance("UICorner", {
			CornerRadius = UDim.new(1, 0),
			Name = "InlineRounding",
			Parent = FovInline,
		})

		Inline = CreateInstance("UIStroke", {
			Parent = FovInline,
			Enabled = true,
			Transparency = 0,
			Color = Color3.fromRGB(255, 255, 255),
			ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual,
			LineJoinMode = Enum.LineJoinMode.Round,
			Thickness = 2,
		})

		Gradient = CreateInstance("UIGradient", {
			Color = ColorSequence.new({
				ColorSequenceKeypoint.new(0.00, Cheat.Options.SilentAim.Fov.Gradient_clr1),
				ColorSequenceKeypoint.new(1.00, Cheat.Options.SilentAim.Fov.Gradient_clr1),
			}),
			Name = "Gradient",
			Parent = Inline,
			Rotation = 90,
		})
		local ReplicatedStorage = game:GetService("ReplicatedStorage")
		local BulletModule = require(ReplicatedStorage.Modules.FPS.Bullet)
		local OldCreateBullet = BulletModule.CreateBullet

		BulletModule.CreateBullet = function(...)
			local Args = { ... }
			local AimPart = Args[5]
			local oldCFrame = AimPart and AimPart.CFrame
			local timestamp = os.date("%X")
			if Cheat.Options.SilentAim.Enabled and Cheat.Options.SilentAim.EnabledKB then
				local TargetCharacter = GetClosestPlayer()
				if TargetCharacter then
					local TargetPart

					if Cheat.Options.SilentAim.HitChance.ClosestPart then
						TargetPart = GetClosestHitPart(TargetCharacter)
					else
						TargetPart = TargetCharacter:FindFirstChild(Cheat.Options.SilentAim.HitPart)
							or TargetCharacter:FindFirstChild("HumanoidRootPart")
							or TargetCharacter:FindFirstChild("Head")
					end

					if TargetPart and AimPart and AimPart:IsA("BasePart") then
						local ProjectileSpeed = ToNumber(GetBullet("MuzzleVelocity"))
						local ProjectileDropValue = ToNumber(GetBullet("ProjectileDrop"))

						local PredictedAimPoint = Cheat.Options.SilentAim.Prediction.Enabled
								and Predict(TargetPart, AimPart.Position, ProjectileSpeed, ProjectileDropValue)
							or TargetPart.Position

						local Dropped = Cheat.Options.SilentAim.Prediction.Enabled
								and PredictionDrop(
									AimPart.Position,
									TargetPart.Position,
									ProjectileSpeed,
									ProjectileDropValue
								)
							or Vector3.new(0, 0, 0)

						local ShouldHit = CalculateHitChance()

						if Cheat.Options.SilentAim.HitChance.Mode == "Activation" then
							if ShouldHit then
								AimPart.CFrame = CFrame.new(
									AimPart.Position,
									Cheat.Options.SilentAim.Prediction.Enabled and (PredictedAimPoint + Dropped)
										or TargetPart.Position
								)
							end
						elseif Cheat.Options.SilentAim.HitChance.Mode == "Force Miss" then
							if ShouldHit then
								AimPart.CFrame = CFrame.new(
									AimPart.Position,
									Cheat.Options.SilentAim.Prediction.Enabled and (PredictedAimPoint + Dropped)
										or TargetPart.Position
								)
							else
								local MissPos = TargetPart.Position + Vector3.new(-10, -5, 6)
								AimPart.CFrame = CFrame.new(AimPart.Position, MissPos)
							end
						else
							AimPart.CFrame = CFrame.new(AimPart.Position, PredictedAimPoint + Dropped)
						end
					end
				end
			end
			if Cheat.Options.GunMods and Cheat.Options.GunMods.doubleTap then
				local doubleArgs = table.clone(Args)
				task.spawn(function()
					OldCreateBullet(table.unpack(doubleArgs))
				end)
			end
			local ok, r1, r2, r3, r4 = pcall(function()
				return OldCreateBullet(table.unpack(Args))
			end)

			if AimPart and oldCFrame then
				AimPart.CFrame = oldCFrame
			end

			if not ok then
				warn(("createbullet failed"):format(timestamp, tostring(r1)))
				return nil, nil, nil, nil
			end
			if Manipulation.Enabled and Manipulation.Bind then
				State.IsShooting = true
				wait(0.5)
				State.IsShooting = false
			end
			return r1, r2, r3, r4
		end

		Cheat.Connections.AimConnection = RunService.PreRender:Connect(function()
			if not Cheat.Options.Aimbot.EnabledKB then
				Cheat.IsLocked = false
				Cheat.LockedTarget = nil
				return
			end

			if not Cheat.IsLocked then
				local Target, distnce = GetClosestPlayer()
				if Target then
					Cheat.LockedTarget = Target
					Cheat.IsLocked = true
				else
					return
				end
			end

			local TargetCharacter = Cheat.LockedTarget
			if not TargetCharacter then
				Cheat.IsLocked = false
				Cheat.LockedTarget = nil
				return
			end

			local ProjectileSpeed = ToNumber(GetBullet("MuzzleVelocity"))
			if not ProjectileSpeed then
				return
			end

			local ProjectileDropValue = ToNumber(GetBullet("ProjectileDrop"))
			if not ProjectileDropValue then
				return
			end

			local HitPart = TargetCharacter:FindFirstChild(Cheat.Options.Aimbot.HitPart)

			if Cheat.Options.Aimbot.ClosestPart then
				HitPart = GetClosestHitPart(TargetCharacter)
			else
				HitPart = TargetCharacter:FindFirstChild(Cheat.Options.Aimbot.HitPart)
			end

			local PredictedAimPoint = HitPart.Position
			local TDropped =
				PredictionDrop(Camera.CFrame.Position, HitPart.CFrame.Position, ProjectileSpeed, ProjectileDropValue)

			if Cheat.Options.Aimbot.Prediction.Enabled then
				PredictedAimPoint = Predict(HitPart, Camera.CFrame.Position, ProjectileSpeed, ProjectileDropValue)
					+ Vector3.new(0, TDropped, 0)
			else
				PredictedAimPoint = HitPart.Position
			end

			if not HitPart then
				Cheat.IsLocked = false
				Cheat.LockedTarget = nil
				return
			end

			local targetCFrame = (CFrame.new(Camera.CFrame.Position, PredictedAimPoint))

			if Cheat.Options.Aimbot.Smoothing.Enabled then
				Camera.CFrame = Camera.CFrame:lerp(targetCFrame, (Cheat.Options.Aimbot.Smoothing.Amount / 100))
			else
				Camera.CFrame = targetCFrame
			end
		end)

		Cheat.Connections.CameraChildAdded = Camera.ChildAdded:Connect(function(Model)
			if Model:IsA("Model") and Model.Name == "ViewModel" then
				local ViewModelHumanoid = Model:WaitForChild("Humanoid")
				if ViewModelHumanoid then
					if Cheat.Connections.ViewModelHumanoidAnimationPlayed then
						Cheat.Connections.ViewModelHumanoidAnimationPlayed:Disconnect()
					end
					Cheat.Connections.ViewModelHumanoidAnimationPlayed = ViewModelHumanoid.AnimationPlayed:Connect(
						function(Track)
							if Track.Name == "Equip" and Cheat.Options.GunMods.InstantEquip.Value then
								Track:AdjustSpeed(9e9)
							end
							--if Track.Name:lower():find("reload") and Cheat.Options.GunMods.InstantReload.Value then
							--	local EquippedItem =
							--		ReplicatedStorage.Players[Client.Name].Status.GameplayVariables.EquippedTool.Value
							--	if EquippedItem then
							--		local ItemProperties = EquippedItem:FindFirstChild("ItemProperties")
							--		if ItemProperties and ItemProperties:GetAttribute("MaxLoadedAmmo") then
							--			Track:AdjustSpeed(5)
							--		else
							--			Track:AdjustSpeed(25)
							--		end
							--	end
							--end
						end
					)
				end
			end
		end)

		local ClientName = Lps.Name
		local Sprinting = ReplicatedStorage.Players[ClientName].Status.GameplayVariables.Sprinting
		local MT = getrawmetatable(game)
		setreadonly(MT, false)

		Cheat.Connections.Namecall = hookmetamethod(
			game,
			"__namecall",
			newcclosure(function(self, ...)
				local Args = { ... }
				local Method = getnamecallmethod()

				if not checkcaller() then
					if Method == "InvokeServer" and self.Name == "Reload" and Cheat.Options.GunMods.InstantReload.Value then
						if Args[2] then
							Args[1] = nil
							Args[2] = 0 / 0
							Args[3] = nil
						end
					end

					if
						Method == "FireServer"
						and tostring(self) == "ProjectileInflict"
						and Cheat.Options.GunMods.InstaBullet
					then
						if type(Args[3]) == "number" and Args[3] >= 0 and Args[3] <= 10 then
							coroutine.wrap(function()
								coroutine.yield()
							end)()
							return nil
						end
						Args[4] = 0 / 0
						return Cheat.Connections.Namecall(self, unpack(Args))
					end

					if
						Method == "Raycast"
						and Cheat.Options.GunMods.InstaBullet
						and getinfo(3).short_src == "ReplicatedStorage.Modules.FPS.Bullet"
					then
						if Cheat.storage.lastHitPart and Cheat.storage.lastHitPart.CFrame.Position then
							Args[2] = (Cheat.storage.lastHitPart.CFrame.Position - Args[1])
							return Cheat.Connections.Namecall(self, unpack(Args))
						end
					end
				end

				if
					Method == "GetAttribute"
					and self == Sprinting
					and Args[1] == "Value"
					and Cheat.Options.GunMods.NoSprintOffsett
				then
					return false
				end

				if Method == "FireServer" and self.Name == "UpdateTilt" then
					if Cheat.rage.AntiAim.Pitchenabled then
						if Cheat.rage.AntiAim.pitchBase == "Zero" then
							Args[1] = 0
						elseif Cheat.rage.AntiAim.pitchBase == "Up" then
							Args[1] = 1
						elseif Cheat.rage.AntiAim.pitchBase == "Down" then
							Args[1] = -1
						elseif Cheat.rage.AntiAim.pitchBase == "Custom" then
							Args[1] = Cheat.rage.AntiAim.customVal
						elseif Cheat.rage.AntiAim.pitchBase == "Jitter" then
							Args[1] = getRandomJitter(Cheat.rage.AntiAim.Jitter1, Cheat.rage.AntiAim.Jitter2)
						elseif Cheat.rage.AntiAim.pitchBase == "Random" then
							Args[1] = math.random() * 2 - 1
						end
						Args[2] = nil
						Args[3] = 0
						Args[4] = 0
					end
				end

				return Cheat.Connections.Namecall(self, unpack(Args))
			end)
		)

		setreadonly(MT, true)

		--//  Functions

		local function GetFovPosition()
			local posType = Cheat.Options.SilentAim.Fov.Position

			if posType == "Mouse" then
				return UDim2.new(0, UserInputService:GetMouseLocation().X, 0, UserInputService:GetMouseLocation().Y - 58)
			elseif posType == "Middle" then
				return UDim2.new(0, Camera.ViewportSize.X / 2, 0, (Camera.ViewportSize.Y / 2) - 58)
			elseif posType == "Barrel" then
				local barrel = GetBarrel()

				if barrel then
					if barrel:IsA("Weld") then
						barrel = barrel.Part1 or barrel.Part0
					end

					if barrel and barrel:IsA("BasePart") then
						local target = barrel.Position + barrel.CFrame.LookVector * 1000
						local screenPos, onScreen = Camera:WorldToViewportPoint(target)
						if onScreen then
							return UDim2.new(0, screenPos.X, 0, screenPos.Y - 58)
						end
					else
						warn("barrel func returned sumn else", barrel)
					end
				end

				return UDim2.new(0, Camera.ViewportSize.X / 2, 0, (Camera.ViewportSize.Y / 2) - 58)
			end

			return UDim2.new(0, 0, 0, 0)
		end

		local function ApplyFovGradient()
			Gradient.Color = ColorSequence.new({
				ColorSequenceKeypoint.new(0.00, Cheat.Options.SilentAim.Fov.Gradient_clr1),
				ColorSequenceKeypoint.new(1.00, Cheat.Options.SilentAim.Fov.Gradient_clr2),
			})
		end

		local function ApplyOutlineStyle()
			Outline.Color = Cheat.Options.SilentAim.Fov.Outline_clr
			Outline.Transparency = Cheat.Options.SilentAim.Fov.OutTrans
			Outline.Enabled = Cheat.Options.SilentAim.Fov.Outline
		end

		local function ApplyInlineStyle()
			Inline.Transparency = Cheat.Options.SilentAim.Fov.InTrans
			Inline.Enabled = true
		end

		local function SetFovSize()
			local baseSize = Cheat.Options.SilentAim.Fov.Size * 2
			if Cheat.Options.SilentAim.Fov.Type == "Dynamic" then
				local scale = 70 / Camera.FieldOfView
				baseSize = baseSize * scale
			end
			FovOutline.Size = UDim2.new(0, baseSize, 0, baseSize)
		end

		--// Main

		RunService.PreRender:Connect(function(DeltaTime)
			UpdateRatio()
			local stor, distance, vis = GetClosestPlayer()
			if stor then
				Cheat.storage.lastHitPart = stor:FindFirstChild("Head")
			end

			if Cheat.Options.SilentAim.Fov.Enabled then
				local currentPos = GetFovPosition()
				FovOutline.Position = currentPos

				Inline.Enabled = true
				Outline.Enabled = Cheat.Options.SilentAim.Fov.Outline

				if Cheat.Options.SilentAim.Fov.Visible then
					ApplyFovGradient()
					ApplyOutlineStyle()
					ApplyInlineStyle()
				else
					Inline.Enabled = false
					Outline.Enabled = false
				end

				SetFovSize()
			else
				Inline.Enabled = false
				Outline.Enabled = false
			end

			if Cheat.Options.SilentAim.Fov.Gradient_rotate then
				Gradient.Rotation = (Gradient.Rotation + Cheat.Options.SilentAim.Fov.Rotation_speed * DeltaTime) % 360
			else
				Gradient.Rotation = 180
			end
		end)
		if not Globals.Assets.__ShaderAnimLoop then
			Globals.Assets.__ShaderAnimLoop = true
			Globals.Engine:Thread(function()
				local frameRate = 1 / 30
				local scrollFactor = 0.03
				while true do
					task.wait(frameRate)
					local textures = Globals.Assets.ShaderAnimatedTextures
					if not textures or #textures == 0 then
						continue
					end
					for i = 1, #textures do
						local d = textures[i]
						if typeof(d) ~= "table" then
							--warn("invalid texte at entrance", i)
							continue
						end
						local t = d.texture
						local s = d.scrollSpeed
						if typeof(t) == "Instance" and t:IsA("Texture") and t.Parent and typeof(s) == "number" then
							t.OffsetStudsU += s * scrollFactor
							t.OffsetStudsV += s * scrollFactor
						end
					end
				end
			end)
		end

		local ShaderCache = {}
		if workspace.CurrentCamera then
			Globals.Engine:Connect(
				workspace.CurrentCamera.ChildAdded,
				LPH_JIT_MAX(function()
					Globals.Engine:Thread(function()
						task.wait(0.5)
						for _, Part in next, GetDescendants(workspace.CurrentCamera.ViewModel) do
							if FindFirstAncestor(Part, "Item") then
								if Cheat.Options.Misc.Custom.ShaderEnabled then
									local Texture = Cheat.Options.Misc.Custom.Shader
									local Tiling = Cheat.Options.Misc.Custom.ShaderTile or 1
									local Speed = Cheat.Options.Misc.Custom.ShaderSpeed or 0.1

									if not ShaderCache[Part] then
										ShaderCache[Part] = true

										if Part:IsA("MeshPart") then
											Part.TextureID = ""
											for _, Child in pairs(Part:GetChildren()) do
												if Child:IsA("Texture") and Child.Name == "WeaponShaderTexture" then
													Child:Destroy()
												end
											end

											local Faces = {
												Enum.NormalId.Front,
												Enum.NormalId.Back,
												Enum.NormalId.Left,
												Enum.NormalId.Right,
												Enum.NormalId.Top,
												Enum.NormalId.Bottom,
											}

											for _, Face in pairs(Faces) do
												local Text = Instance.new("Texture")
												Text.Name = "WeaponShaderTexture"
												Text.Face = Face
												Text.Texture = Globals.Assets.Shaders[Texture]
												Text.StudsPerTileU = Tiling
												Text.StudsPerTileV = Tiling
												Text.Parent = Part

												if Speed ~= 0 then
													table.insert(
														Globals.Assets.ShaderAnimatedTextures,
														{ texture = Text, scrollSpeed = Speed }
													)
												end
											end
										end

										if Part:IsA("SurfaceAppearance") then
											Part:Destroy()
										end
									end
								end
							end
						end
					end)
				end)
			)
		end
	end

	--// extra hooks
	ahs = hookmetamethod(game, "__namecall", function(self, ...)
		local method = getnamecallmethod()
		local args = { ... }

		if
			not checkcaller()
			and self.Name == "ProjectileInflict"
			and method == "FireServer"
			and Cheat.Options.GunMods.AlwaysHeadshot
		then
			if typeof(args[1]) == "Instance" then
				args[1] = args[1].Parent.FaceHitBox
				return ahs(self, unpack(args))
			end
		end

		return ahs(self, ...)
	end)
	--
	do -- loader
		local Window2 = Menu:Create({ Name = "Loader", Size = Vector2.new(350, 400), PageAmmount = 1, Single = true })
		local Main = Window2:Page({ Name = "Loader" })
		local Updates = Window2:Page({ Name = "Updates" })
		local Section = Main:Section({ Name = "" })
		local Section2 = Updates:Section({ Name = "" })
		local time = LRM_SecondsLeft / 86400
		local status = "undetected"
		local users = "11"
		local build = "beta"
		local startload = false
		Section:Label({ Name = "User Information", Inlayed = true })
		Section:Label({ Name = "	User: " .. LRM_LinkedDiscordID, Inlayed = true })
		Section:Label({ Name = "	UID: " .. LRM_Uid, Inlayed = true })
		Section:Label({ Name = "	Time Left: " .. time .. " day(s)", inlayed = true })
		Section:Label({ Name = "Cheat", Inlayed = true })
		Section:Label({ Name = "	Game: " .. game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name })
		Section:Label({ Name = "	Status: " .. status })
		Section:Label({ Name = "	Ingame: " .. users })
		Section:Label({ Name = "	Build: " .. build })
		Section:Button({
			Name = "load",
			Callback = function(v)
				Window2:Switch(false)
				wait(0.5)
				Window:Switch(true)
			end,
		})
		Section2:Label({ Name = "Added", Inlayed = true })
		Section2:Label({ Name = "	PD new silent aim", Inlayed = true })
		Section2:Label({ Name = "	New esp for all games", Inlayed = true })
		Section2:Label({ Name = "	New Loader", Inlayed = true })
		Section2:Label({ Name = "	UID System", Inlayed = true })
		Section2:Label({ Name = "	PD Underground Replication", Inlayed = true })
		Section2:Label({ Name = "	New options for fallen noclip", Inlayed = true })
		Section2:Label({ Name = "Removed", Inlayed = true })
		Section2:Label({ Name = "	Detected features", Inlayed = true })
		Section2:Label({ Name = "Changed", Inlayed = true })
		Section2:Label({ Name = "	Fallen bypass", Inlayed = true })
		Window2.Pages[1]:Set(true)
		Window2:Initialise()
	end
