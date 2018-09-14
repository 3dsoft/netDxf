# netDxf
netDxf 2.2.0 Copyright(C) 2009-2018 Daniel Carvajal, Licensed under LGPL
## Description
netDxf은 AutoCAD Dxf 파일을 읽고 쓰기 위해 만들어진 .net 라이브러리입니다.

netDxf는 다음과 같은 형식의 Dxf 파일 버전을 지원하고, text 형식과 binary 형식 모두 지원합니다.

AutoCad2000

AutoCad2004

AutoCad2007

AutoCad2010

AutoCad2013

AutoCad2018

라이브러리는 사용하기 쉽고, 가능한한 복잡하지 않은 방식을 유지하려고 노력했습니다.

예를 들어 레이어, 스타일 또는 선 유형을 정의할때 관련된 테이블 섹션을 모두 채울 필요가 없습니다. 

DxfDocument는 새 항목이 추가 될 때마다 처리합니다.

Dxf에 관한 공식적인 포맷 정보는 [이곳](https://help.autodesk.com/view/OARX/2019/ENU/?guid=GUID-235B22E0-A567-4CF6-92D3-38A2306D73F3)에서 확인할 수 있습니다. 

코드 예제:

```c#
public static void Main()
{
	// 읽고 쓰기 위한 dxf 파일 이름
	string file = "sample.dxf";

	// 기본적으로 AutoCad2000 DXF 버전으로 생성됩니다. 
	DxfDocument dxf = new DxfDocument();
	
	// 선 개체 생성
	Line entity = new Line(new Vector2(5, 5), new Vector2(10, 5));
	
	// 개체를 추가
	dxf.AddEntity(entity);
	
	// 파일에 저장
	dxf.Save(file);

	bool isBinary;
	
	// 지원하지 않는 dxf 버전이 있기 때문에 아래처럼 버전을 체크해서 예외 상황을 처리해야함
	DxfVersion dxfVersion = DxfDocument.CheckDxfFileVersion(file, out isBinary);
	
	// netDxf에서 지원하지 않는 dxf를 체크해서 처리한다.
	if (dxfVersion < DxfVersion.AutoCad2000 || dxfVersion == DxfVersion.Unknown) return;
	
	// dxf 파일 읽기
	DxfDocument loaded = DxfDocument.Load(file);
	if(loaded == null) return;

        // Circles 개체를 모두 찾아서 출력
        foreach (var item in dxf.Circles)
        {
            Debug.Print($"G11 X{(item.Center.X - G92x).ToString("0.0000")} Y{(item.Center.Y - G92y).ToString("0.0000")}");
        }
	
	// Points 개체를 모두 찾아서 출력
	foreach (var item in dxf.Points)
        {
            Debug.Print($"G11 X{(item.Position.X - G92x).ToString("0.0000")} Y{(item.Position.Y - G92y).ToString("0.0000")}");
        }
}
```

## Samples and Demos 
Program.cs 파일 참조

Well, at the moment they are just tests for the work in progress.
## Dependencies and distribution 
* .NET Framework 4.5를 기반으로 System.dll과 System.Drawing.dll만 참조합니다.
## Compiling
컴파일을 하기위해 Visual Studio 2015가 필요합니다.
## Development Status 
Stable. See [changelog.txt](https://github.com/haplokuon/netDxf/blob/master/doc/Changelog.txt) or the [wiki page](https://github.com/haplokuon/netDxf/wiki) for information on the latest changes.
## Supported entities
* 3dFace
* Arc
* Circle
* Dimensions (aligned, linear, radial, diametric, 3 point angular, 2 line angular, and ordinate)
* Ellipse
* Hatch (including Gradient patterns)
* Image
* Insert (block references and attributes)
* Leader
* Line
* LwPolyline (light weight polyline)
* Mesh
* MLine
* MText
* Point
* PolyfaceMesh
* Polyline
* Ray
* Shape
* Solid
* Spline
* Text
* Tolerance
* Trace
* Underlay (DGN, DWF, and PDF underlays)
* Wipeout
* XLine (aka construction line)


모든 개체(entities)를 그룹화 할 수 있습니다. 

모든 개체(entities)와 테이블 개체(object)는 확장 된 데이터 정보를 포함 할 수 있습니다. 

AutoCAD 테이블 개체는 Insert(블록 참조)하는 방식으로 포함될것입니다.

복잡하고 단순한 선 유형을 모두 지원합니다.

이 라이브러리는 Regions 및 3dSolids와 같은 일부 엔티티를 읽을 수 없습니다.
