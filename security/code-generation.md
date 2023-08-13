# Auth code
```
    public static string GenerateCode(int length = 24)
    {
        var fileSizeInByte = (int)Math.Ceiling((double)length / 4) * 3;
        var buff = new byte[fileSizeInByte];
        RandomNumberGenerator.Fill(buff);
        return Convert.ToBase64String(buff);
    }
```