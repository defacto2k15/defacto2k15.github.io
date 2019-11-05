# Szrafowanie

Metodami szrafowania zajmowałem się w ramach tworzenia pracy magisterskiej pt. „_Algorytm szrafowania w przestrzeni obrazu zachowujący spójność czasową_” TODO link. Zaimplementowałem sześć istniejących rozwiązań z artykułów naukowych oraz dwie nowe metody. Sposoby szrafowania w zautomatyzowany sposób porównywałem pod względem spójności czasowej tworzonej animacji oraz wydajności. 

Projekt Unity zawarty w [repozytorium](https://github.com/defacto2k15/PwMgr) zawiera wszelkie zasoby oraz kod pozwalający na własnoręcznie uruchomienie sceny wykorzystującej szrafowanie dowolnym rozwiązaniem. 
Każda metoda ma  przypisany skrót wykorzystywany w nazwach klas i nazwach shaderów do danego sposobu szrafowania przypisanych.
Wyróżnić należy zestaw skryptów i obiektów, które mogą być używane w danej metodzie. Podaje ścieżki do tych elementów, kiedy są wykorzystywane.

  - Skrypt obiektu – MonoBehaviour przypisany do obiektu, który jest szrafowany.
  - Skrypt kamery – MonoBehaviour przypisany do kamery renderującej scenę. Zwykle modyfikuje proces rysowania tak, aby umożliwić post-processing
  - Materiał obiektu – Materiał, czyli shader + wartości parametrów, które rysują szrafowany obiekt na ekranie w pierwszym, a czasami jedynym przebiegu renderowania (pass).   
  - Materiał post-procesingu – materiał wykorzystywany pod koniec procesu rysowania klatki, który odczytuje i modyfikuje piksele już wcześniej wyrenderowanej ramki. W przeciwieństwie do materiału obiektu nie operuje na wierzchołkach i nie rysuje jedynie niektórych obiektów. 

Shadery używają techniki wielu celów renderowania (MRT), dzięki czemu pixel shader zapisuje kolory do kilku ramek. Wykorzystuje technikę tą przede wszystkim do zbierania danych wykorzystywanych dalej do porównań. Niektóre metody szrafowania wymagają jednak zapisywania dodatkowych danych w rezultacie działania shadera obiektu, do czego też używam MRT.

W przypadku niektórych rozwiązań materiał obiektu potrzebuje więcej informacji niż tych domyślnie przypisanych do mesha jak normalne, styczne itd.  W takim przypadku generuje wcześniej bufor, w którym umieszczone są wymagane dane. Przy uruchomieniu projektu skrypt  [MasterCachedShaderBufferInjectOC](https://github.com/defacto2k15/PwMgr/blob/master/Assets/NPR/DataBuffers/MasterCachedShaderBufferInjectOC.cs) odczytuje je z dysku i przesyła do shadera jako `StructuredBuffer`, z którego poszczególne elementy są odczytywane z wykorzystaniem identyfikatora wierzchołka bądź trójkąta. 

W projektach występuje ruchoma kamera. Kontrolować można ją za pomocą
  - **WSAD**, oraz **E** i **Q** kontroluje położenie w przestrzeni. 
  - Myszka – Składowe Pitch i Yaw obrotu kamery
  - **Y** i **T** – Składowa Roll obrotu kamery  

<dl>
              <video
                id="gif-mp4"
                poster="https://media.giphy.com/media/cjKwheOfSEGdsw0gEV/200_s.gif"
                style="margin:0;padding:0"
                width="480"
                height="270"
                autoplay
                loop>
                <source src="https://media.giphy.com/media/cjKwheOfSEGdsw0gEV/giphy.mp4" type='video/mp4; codecs="avc1.42E01E, mp4a.40.2"'></source>
                <img src="https://media.giphy.com/media/cjKwheOfSEGdsw0gEV/giphy.gif" title="Your browser does not support the mp4 video codec." />
            </video>
  <dt>Definition list</dt>
  <dd>Is something people use sometimes.</dd>

  <dt>Markdown in HTML</dt>
  <dd>Does *not* work **very** well. Use HTML <em>tags</em>.</dd>
</dl>
            <video
                id="gif-mp4"
                poster="https://media.giphy.com/media/cjKwheOfSEGdsw0gEV/200_s.gif"
                style="margin:0;padding:0"
                width="480"
                height="270"
                autoplay
                loop>
                <source src="https://media.giphy.com/media/cjKwheOfSEGdsw0gEV/giphy.mp4" type='video/mp4; codecs="avc1.42E01E, mp4a.40.2"'></source>
                <img src="https://media.giphy.com/media/cjKwheOfSEGdsw0gEV/giphy.gif" title="Your browser does not support the mp4 video codec." />
            </video>

# Zaimplementowane metody
## Tam - Mapy tonalne
  - Projekt [mf-tam](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Projects/mf/mf-tam.unity)
  - Materiał obiektu afTam, [mf_Tam.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/mf_Tam.shader)

Utworzone na podstawie pracy _E. Praun, H. Hoppe, M. Webb i A. Finkelstein, „Real-time Hatching_”. Nie zaimplementowałem jednak proponowanego w niej sposobu obrotu kresek za pomocą nakładających się łat. 

## Breslav 
  - Projekt [mf-Breslav](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Projects/mf/mf-Breslav.unity)
  - Materiał obiektu afBreslav,  [mf_Breslav.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/mf_Breslav.shader)
  - Skrypt obiektu [BreslavObjectDebugOC](https://github.com/defacto2k15/PwMgr/blob/master/Assets/NPR/Filling/Breslav/BreslavObjectDebugOC.cs)

Utworzone na podstawie pracy _S. Breslav, K. Szerszen, L. Markosian, P. Barla i J. Thollot, „Dynamic 2D Patterns for Shading 3D Scenes”_, bez zaimplementowanego dzielenia obiektu na łaty.

## Jordane
  - Projekt [mf-Jordane](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Projects/mf/mf-Jordane.unity) 
  - Materiał obiektu afJordane,  [mf_jordane.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/mf_jordane.shader)
  
Utworzone na podstawie pracy _J. Suarez, F. Belhadj i V. Boyer, „Real-time 3D Rendering with Hatching_.

## Szecsi
  - Projekt [mf-Szecsi](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Projects/mf/mf-Szecsi.unity)
  - Materiał obiektu afSzecsi, [mf-szecsi.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/mf-szecsi.shader)
  - Skrypt obiektu [NprSzecsiDebugGo](https://github.com/defacto2k15/PwMgr/blob/master/Assets/NPR/Filling/Szecsi/NprSzecsiDebugGo.cs)

Utworzone na podstawie pracy _T. U. László Szécsi Marcell Szirányi, „Improving Texture-based NPR”_.	

## Wolowski
  - Projekt [mf-Wolowski](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Projects/mf/mf-Wolowski.unity)
  - Materiał obiektu afWolowski, [mf_Wolowski.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/mf_Wolowski.shader)

Utworzone na podstawie pracy magisterskiej _K. Wołowski, „Stylized Shading Algorithms_. 	

## TamIss
  - Projekt [mf-tamiss](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Projects/mf/mf-tamiss.unity)
  - Skrypt kamery: [TamIssPostProcessingDirectorOC](https://github.com/defacto2k15/PwMgr/blob/master/Assets/NPR/Filling/TamID/TamIssPostProcessingDirectorOC.cs)
  - Materiały post-processingu		
      - afTamIssFragmentGathering [mf_TamIss_FragmentGathering.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/mf_TamIss_FragmentGathering.shader)
      - FillingTamIdBufferAggregateRendererMat [sandbox_fillingTamIdBufferAggreagateRenderer.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/sandbox_fillingTamIdBufferAggreagateRenderer.shader)
      - FillingTamIdBufferMaxMinRendererMat [sandbox_fillingTamIdBufferMinMaxRenderer.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/sandbox_fillingTamIdBufferMinMaxRenderer.shader)

Utworzone na podstawie  _L. Szécsi, M. Szirányi i A. Kacsó, „Tonal Art Maps with Image Space Strokes”_.

## MMGeometric

  - Projekt [mf-mmGeometric](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Projects/mf/mf-mmGeometric.unity)
  - Skrypt obiektu [MyMethodObjectDebugOC](https://github.com/defacto2k15/PwMgr/blob/master/Assets/NPR/Filling/MM/MyMethodObjectDebugOC.cs)
  - Materiał obiektu [sandbox_fillingMM3DSeedsDebug.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/sandbox_fillingMM3DSeedsDebug.shader)
  - Skrypt kamery [MyMethodPostProcessingDirectorOC](https://github.com/defacto2k15/PwMgr/blob/master/Assets/NPR/Filling/MM/MyMethodPostProcessingDirectorOC.cs)
  - Materiały post-processingu 
      - filling_MMStage1Mat [sandbox_fillingMMStage1Renderer.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/sandbox_fillingMMStage1Renderer.shader)
      - filling_MMGeometryAlphaRenderingMat [sandbox_fillingMMGeometryAlphaRenderer.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/sandbox_fillingMMGeometryAlphaRenderer.shader)

Nowatorskie rozwiązanie mojego autorstwa. Poszczególne kreski rysowane są jako prostokąty umieszczane na powierzchni ekranu. Nie wymaga definiowania współrzędnych UV. Parametryzowany poziom szczegółowości [nieskończony zoom] tak jak rozwiązanie Szecsi. 

## MMStandard

  - Projekt [mf-mmStandard](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Projects/mf/mf-mmStandard.unity)
  - Skrypt obiektu [MyMethodObjectDebugOC](https://github.com/defacto2k15/PwMgr/blob/master/Assets/NPR/Filling/MM/MyMethodObjectDebugOC.cs)
  - Materiał obiektu [sandbox_fillingMM3DSeedsDebug.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/sandbox_fillingMM3DSeedsDebug.shader)
  - Skrypt kamery [MyMethodPostProcessingDirectorOC](https://github.com/defacto2k15/PwMgr/blob/master/Assets/NPR/Filling/MM/MyMethodPostProcessingDirectorOC.cs)
  - Materiały post-processingu 
      - filling_MMStage1Mat [sandbox_fillingMMStage1Renderer.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/sandbox_fillingMMStage1Renderer.shader)
      - fillingMM_SSGM_DebugRendererMap [sandbox_fillingMMStrokeSeedGridMapDebugRenderer.shader](https://github.com/defacto2k15/PwMgr/blob/master/Assets/Resources/shaders/sandbox_fillingMMStrokeSeedGridMapDebugRenderer.shader)

Alternatywna wersja mojego rozwiązania. Kreski rysowane są bezpośrednio w pikselowym shaderze, z pominięciem prostokątnej reprezentacji geometrycznej.

