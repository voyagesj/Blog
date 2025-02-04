메인 스레드에 영향을 받지 않고 Loading 중 표시하기.

```c#
public bool NowLoading { get; set; } = false;

public async Task<string> MainTask()
{
    //... 생략

    //Loading 처리
    NowLoading = true;
    NotiServiceLoading();

    //... 생략

    if (response.IsSuccessStatusCode)
    {
        //Loading 완료
        NowLoading = false;
        
        //... 생략
    }
    
    Console.WriteLine((DateTime.Now - StartTime).TotalSeconds);
    await Task.Delay(1);
    return "";
}

public async void NotiServiceLoading()
{
    do
    {
        var x = Console.GetCursorPosition().Top;
        Console.SetCursorPosition(0, x);
        switch (DateTime.Now.Second % 4)
        {
            case 0: Console.Write("―"); break;
            case 1: Console.Write("＼"); break;
            case 2: Console.Write("｜"); break;
            default: Console.Write("／"); break;
        }
        await Task.Delay(1000); 
        
    } while (NowLoading);
    if (!NowLoading) { Console.WriteLine("LoadCompleted"); }
}
```
