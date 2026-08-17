local function base64_decode(data)
    local b = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'
    data = string.gsub(data, '[^'..b..'=]', '')
    local result = ""
    for i = 1, #data, 4 do
        local chunk = data:sub(i, i+3)
        local n = 0
        for j = 1, 4 do
            local c = chunk:sub(j, j)
            if c ~= '=' then
                n = n * 64 + (b:find(c) - 1)
            end
        end
        for j = 1, 3 do
            if chunk:sub(j, j) ~= '=' then
                result = result .. string.char(bit32.rshift(n, 8 * (3 - j)) % 256)
            end
        end
    end
    return result
end

local encoded = game:HttpGet("https://raw.githubusercontent.com/AA-RD-K2/Ultra/main/ultra.lua")
local decoded = base64_decode(encoded)
loadstring(decoded)()