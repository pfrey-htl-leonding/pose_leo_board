﻿# LeoBoard

A utility library for creating simple, grid-based UI application like Minesweeper, Chess, etc.

Allows drawing a grid with specified size, optional row and column numbers, and content for each cell.
Left- and right-click events are captured and can be handled to update the cell content.

![sample_run.png](https://raw.githubusercontent.com/markushaslinger/pose_leo_board/master/sample_run.png)

## Sample usage

```csharp
using System.Text;
using Avalonia.Media;
using LeoBoard;

Board.Initialize(Run, "Foo", 10, 20,
                 clickHandler: HandleClick,
                 drawGridNumbers: true);

return;

void Run()
{
    Console.OutputEncoding = Encoding.UTF8;
    
    Board.SetCellContent(0, 0, "X");
    Board.SetCellContent(1, 1, "Y", Brushes.Red);
    Board.SetCellContent(2, 2, "Z", Brushes.Blue);
    
    Console.WriteLine(Board.GetCellContent(2, 2));
    
    Board.ShowMessageBox("Hello World!");
    
    Console.ReadKey();
}

void HandleClick(int row, int col, bool leftClick, bool ctrlKeyPressed)
{
    string ctrlState = ctrlKeyPressed ? " while pressing the Ctrl key" : string.Empty;
    Console.WriteLine($"Clicked cell ({row}, {col}) with {(leftClick ? "left" : "right")} mouse button{ctrlState}");
    if (leftClick)
    {
        Board.SetCellContent(row, col, "A", Brushes.Green);
    }
    else
    {
        Board.SetCellContent(row, col, "B", Brushes.Purple);
    }
}
```

- It is necessary to pass a method reference (`Run` in the above sample) to the initialization method, because to also support macOS we have to use the main thread as UI thread and spin up a second thread for the console interaction and actual 'main' logic. This complexity is hidden from the students as good as possible, but this small requirement remains for technical reasons.
- The `Console.ReadKey()` call is required to keep the hosting console application alive
- Allow for a few seconds for the window to appear (some async initialization is happening)