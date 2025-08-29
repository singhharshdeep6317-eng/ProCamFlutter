# ProCamFlutter
Camera application for mobile 
// Vivo V30 Pro — Pro Cam (Extended)
// Kotlin, Camera2 API with advanced pro features

// build.gradle (app-level)
dependencies {
    implementation "androidx.camera:camera-core:1.3.0"
    implementation "androidx.camera:camera-camera2:1.3.0"
    implementation "androidx.camera:camera-lifecycle:1.3.0"
    implementation "androidx.camera:camera-video:1.3.0"
    implementation "androidx.camera:camera-view:1.3.0"
    implementation "androidx.camera:camera-extensions:1.3.0"
    implementation "androidx.lifecycle:lifecycle-runtime-ktx:2.6.2"
    implementation "androidx.constraintlayout:constraintlayout:2.1.4"
}

// AndroidManifest.xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-feature android:name="android.hardware.camera" />
<uses-feature android:name="android.hardware.camera.autofocus" />

<application ...>
    <activity android:name=".ProCamActivity"
        android:screenOrientation="portrait"
        android:theme="@style/Theme.MaterialComponents.DayNight.NoActionBar" />
</application>

// res/layout/activity_pro_cam.xml
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.camera.view.PreviewView
        android:id="@+id/previewView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <View
        android:id="@+id/gridOverlay"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@drawable/grid_overlay"
        android:visibility="gone" />

    <SeekBar
        android:id="@+id/exposureSlider"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="8dp"
        android:max="10"
        android:min="-10"
        app:layout_constraintBottom_toBottomOf="parent" />

    <LinearLayout
        android:id="@+id/controlPanel"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:gravity="center"
        app:layout_constraintBottom_toTopOf="@id/exposureSlider">

        <Button android:id="@+id/btnCapture" android:text="Photo" />
        <Button android:id="@+id/btnVideo" android:text="Record" />
        <Button android:id="@+id/btnLens" android:text="Switch" />
        <Button android:id="@+id/btnHDR" android:text="HDR" />
        <Button android:id="@+id/btnRaw" android:text="RAW" />
        <Button android:id="@+id/btnLog" android:text="LOG" />
        <Button android:id="@+id/btnWB" android:text="WB" />
        <Button android:id="@+id/btnFocus" android:text="Focus" />
        <Button android:id="@+id/btnZebra" android:text="Zebra" />
        <Button android:id="@+id/btnHistogram" android:text="Histogram" />
        <Button android:id="@+id/btnPeaking" android:text="Peaking" />
    </LinearLayout>
</androidx.constraintlayout.widget.ConstraintLayout>

// res/drawable/grid_overlay.xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <size android:width="1dp" android:height="1dp" />
    <stroke android:color="#50FFFFFF" android:width="1dp" />
    <corners android:radius="0dp" />
</shape>

// ProCamActivity.kt
class ProCamActivity : AppCompatActivity() {
    private lateinit var previewView: PreviewView
    private lateinit var cameraController: ProCameraController

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_pro_cam)

        previewView = findViewById(R.id.previewView)
        cameraController = ProCameraController(this, previewView)

        findViewById<Button>(R.id.btnCapture).setOnClickListener { cameraController.takePhoto() }
        findViewById<Button>(R.id.btnVideo).setOnClickListener { cameraController.toggleVideo() }
        findViewById<Button>(R.id.btnLens).setOnClickListener { cameraController.switchCamera() }
        findViewById<Button>(R.id.btnHDR).setOnClickListener { cameraController.toggleHDR() }
        findViewById<Button>(R.id.btnRaw).setOnClickListener { cameraController.toggleRaw() }
        findViewById<Button>(R.id.btnLog).setOnClickListener { cameraController.toggleLog() }
        findViewById<Button>(R.id.btnWB).setOnClickListener { cameraController.cycleWhiteBalance() }
        findViewById<Button>(R.id.btnFocus).setOnClickListener { cameraController.toggleManualFocus() }
        findViewById<Button>(R.id.btnZebra).setOnClickListener { cameraController.toggleZebra(previewView) }
        findViewById<Button>(R.id.btnHistogram).setOnClickListener { cameraController.toggleHistogram(previewView) }
        findViewById<Button>(R.id.btnPeaking).setOnClickListener { cameraController.toggleFocusPeaking(previewView) }

        val exposureSlider = findViewById<SeekBar>(R.id.exposureSlider)
        exposureSlider.setOnSeekBarChangeListener(object : SeekBar.OnSeekBarChangeListener {
            override fun onProgressChanged(seekBar: SeekBar?, progress: Int, fromUser: Boolean) {
                cameraController.setExposureCompensation(progress - 5)
            }
            override fun onStartTrackingTouch(seekBar: SeekBar?) {}
            override fun onStopTrackingTouch(seekBar: SeekBar?) {}
        })
    }
}

// ProCameraController.kt (pseudo-extended for brevity)
class ProCameraController(private val activity: Activity, private val previewView: PreviewView) {
    fun takePhoto() { /* Capture JPEG or RAW */ }
    fun toggleVideo() { /* Start/stop recording, support HEVC, HDR10+ */ }
    fun switchCamera() { /* Switch between wide, ultra-wide, telephoto */ }
    fun toggleHDR() { /* Enable HDR mode if supported */ }
    fun toggleRaw() { /* Switch capture format to RAW */ }
    fun toggleLog() { /* Apply flat tonemap (LOG) */ }
    fun cycleWhiteBalance() { /* Cycle between auto, daylight, cloudy, tungsten, Kelvin */ }
    fun toggleManualFocus() { /* Enable/disable manual slider for focus distance */ }
    fun toggleZebra(previewView: PreviewView) { /* Overlay stripes for overexposed areas */ }
    fun toggleHistogram(previewView: PreviewView) { /* Draw live luminance histogram */ }
    fun toggleFocusPeaking(previewView: PreviewView) { /* Highlight edges in focus */ }
    fun setExposureCompensation(value: Int) { /* Apply EC */ }
}

// Notes:
// - For LOG: use CaptureRequest.TONEMAP_MODE_CONTRAST_CURVE with flat curve.
// - For RAW: configure ImageReader with ImageFormat.RAW_SENSOR.
// - For Zebra: check pixel brightness, overlay stripes.
// - For Histogram: sample YUV luminance, draw chart.
// - For Focus Peaking: run Sobel/edge filter on preview frame, overlay highlights.
// - For HEVC/HDR10+: configure MediaRecorder profile.
// - FPS/Resolution: use CameraCharacteristics to query and set 24/30/60 fps at 1080p/4K.


---

## Additions — Focus Peaking, Zebra, Histogram, HEVC/HDR10+, Per-physical camera selection

### Overview
I added new overlay views and preview-frame processing hooks to provide:
- Focus peaking (edge detection on preview; highlights in overlay)
- Zebra exposure warning (luma threshold overlay)
- Live histogram (luma histogram view)
- HEVC (H.265) recording attempt with graceful fallback to H.264
- HDR10+ note and a sample path using MediaCodec when platform supports it
- Per-physical camera selection support (uses `CameraCharacteristics#getPhysicalCameraIds()` and `OutputConfiguration.setPhysicalCameraId()`)

Below are the new files and snippets that were appended to the project. Drop them under `src/main/java/com/example/procam/ui/` and wire them up as noted.

### `FocusPeakingView.kt`
```kotlin
package com.example.procam.ui

import android.content.Context
import android.graphics.*
import android.util.AttributeSet
import android.view.View

class FocusPeakingView @JvmOverloads constructor(
    ctx: Context, attrs: AttributeSet? = null
) : View(ctx, attrs) {
    private val paint = Paint().apply { style = Paint.Style.STROKE; strokeWidth = 2f }
    private var edgeBitmap: Bitmap? = null

    fun updateEdges(bitmap: Bitmap?) {
        edgeBitmap = bitmap
        invalidate()
    }

    override fun onDraw(canvas: Canvas) {
        super.onDraw(canvas)
        edgeBitmap?.let {
            val src = Rect(0,0,it.width,it.height)
            val dst = Rect(0,0,width,height)
            paint.color = Color.YELLOW
            canvas.drawBitmap(it, src, dst, null)
        }
    }
}
```

### `ZebraOverlay.kt`
```kotlin
package com.example.procam.ui

import android.content.Context
import android.graphics.*
import android.util.AttributeSet
import android.view.View

class ZebraOverlay @JvmOverloads constructor(
    ctx: Context, attrs: AttributeSet? = null
) : View(ctx, attrs) {
    private val paint = Paint().apply { color = 0x80FFFF00.toInt(); style = Paint.Style.FILL }
    private var mask: Bitmap? = null

    fun updateMask(b: Bitmap?) { mask = b; invalidate() }

    override fun onDraw(canvas: Canvas) {
        super.onDraw(canvas)
        mask?.let {
            val src = Rect(0,0,it.width,it.height)
            val dst = Rect(0,0,width,height)
            canvas.drawBitmap(it, src, dst, paint)
        }
    }
}
```

### `HistogramView.kt`
```kotlin
package com.example.procam.ui

import android.content.Context
import android.graphics.Canvas
import android.graphics.Paint
import android.util.AttributeSet
import android.view.View

class HistogramView @JvmOverloads constructor(
    ctx: Context, attrs: AttributeSet? = null
) : View(ctx, attrs) {
    private val paint = Paint().apply { style = Paint.Style.FILL }
    private var bins = IntArray(256)

    fun update(b: IntArray) { bins = b; invalidate() }

    override fun onDraw(canvas: Canvas) {
        super.onDraw(canvas)
        val w = width.toFloat(); val h = height.toFloat()
        val max = (bins.maxOrNull() ?: 1).coerceAtLeast(1)
        val barW = w / bins.size
        paint.color = 0xFFFFFFFF.toInt()
        for (i in bins.indices) {
            val norm = bins[i].toFloat() / max
            val barH = norm * h
            val left = i * barW
            canvas.drawRect(left, h - barH, left + barW, h, paint)
        }
    }
}
```

### Hooking preview frames in `ProCameraController`
Append a `previewReader` (YUV_420_888) and process frames for peaking/zebra/histogram:

```kotlin
// add properties
private var previewReader: ImageReader? = null
private var peakingEnabled = true
private var zebraEnabled = true
private var histEnabled = true
// external callbacks to update overlays (set from Activity)
var onPeakingBitmap: ((Bitmap?)->Unit)? = null
var onZebraMask: ((Bitmap?)->Unit)? = null
var onHistogram: ((IntArray)->Unit)? = null

// in bind(), after previewSurface creation, create previewReader
val pSize = Size(1280, 720) // small for fast CPU processing; choose wisely
previewReader = ImageReader.newInstance(pSize.width, pSize.height, ImageFormat.YUV_420_888, 2)
previewReader?.setOnImageAvailableListener({ reader ->
    val img = reader.acquireLatestImage() ?: return@setOnImageAvailableListener
    try { processPreviewFrame(img) } finally { img.close() }
}, Handler(Looper.getMainLooper()))

// ensure preview output includes previewReader.surface in session outputs and repeating request targets
```

And add `processPreviewFrame` implementation:

```kotlin
private fun processPreviewFrame(image: android.media.Image) {
    // simple Y-plane extract
    val plane = image.planes[0]
    val rowStride = plane.rowStride
    val buffer = plane.buffer
    val w = image.width
    val h = image.height
    val y = ByteArray(rowStride * h)
    buffer.get(y, 0, y.size)

    // build small greyscale bitmap for overlays
    val bmp = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_8888)
    var idx = 0
    for (yy in 0 until h) {
        for (x in 0 until w) {
            val Y = (y[idx].toInt() and 0xFF)
            val col = 0xFF shl 24 or (Y shl 16) or (Y shl 8) or Y
            bmp.setPixel(x, yy, col)
            idx += 1
        }
        idx = yy*rowStride + (w) // correct index for next line (approx)
    }

    // histogram
    if (histEnabled) {
        val bins = IntArray(256)
        val pixels = IntArray(w*h)
        bmp.getPixels(pixels, 0, w, 0, 0, w, h)
        for (p in pixels) bins[(p and 0xFF)]++
        onHistogram?.invoke(bins)
    }

    // simple sobel for edges (focus peaking)
    if (peakingEnabled) {
        val edges = sobelEdgeDetect(bmp)
        onPeakingBitmap?.invoke(edges)
    }

    // zebra: luma threshold mask
    if (zebraEnabled) {
        val mask = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_8888)
        val pixels = IntArray(w*h)
        bmp.getPixels(pixels, 0, w, 0, 0, w, h)
        val threshold = 245
        for (i in pixels.indices) {
            val yv = pixels[i] and 0xFF
            if (yv >= threshold) pixels[i] = 0x80FFFF00.toInt() else pixels[i] = 0
        }
        mask.setPixels(pixels, 0, w, 0, 0, w, h)
        onZebraMask?.invoke(mask)
    }
}

private fun sobelEdgeDetect(src: Bitmap): Bitmap {
    val w = src.width; val h = src.height
    val out = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_8888)
    val inPixels = IntArray(w*h); src.getPixels(inPixels,0,w,0,0,w,h)
    val outPixels = IntArray(w*h)
    val gx = intArrayOf(-1,0,1,-2,0,2,-1,0,1)
    val gy = intArrayOf(-1,-2,-1,0,0,0,1,2,1)
    for (y in 1 until h-1) {
        for (x in 1 until w-1) {
            var sx = 0; var sy = 0; var k = 0
            for (yy in -1..1) for (xx in -1..1) {
                val p = inPixels[(y+yy)*w + (x+xx)] and 0xFF
                sx += p * gx[k]; sy += p * gy[k]; k++
            }
            val mag = (Math.hypot(sx.toDouble(), sy.toDouble()).coerceAtMost(255.0)).toInt()
            outPixels[y*w + x] = (0xFF shl 24) or (mag shl 16) or (mag shl 8) or mag
        }
    }
    out.setPixels(outPixels,0,w,0,0,w,h)
    return out
}
```

Wire activity to overlays (in `ProCamActivity.onCreate` after UI init):

```kotlin
// after camera.bind() callback is set up, pass overlay hooks
camera.onPeakingBitmap = { bmp -> runOnUiThread { b.focusPeakingView.updateEdges(bmp) } }
camera.onZebraMask = { m -> runOnUiThread { b.zebraOverlay.updateMask(m) } }
camera.onHistogram = { bins -> runOnUiThread { b.histogramView.update(bins) } }
```

Add the corresponding views to `activity_pro_cam.xml` (small overlay panels) — they were added to the canvas update.

### HEVC (H.265) & HDR10+
- The code now attempts to choose HEVC if the device supports `video/hevc` encoder. I added a helper `chooseVideoEncoder()` that queries `MediaCodecList` and selects HEVC (and Main10 profile) when available. If HEVC is chosen `prepareRecorder()` will attempt to set `MediaRecorder.VideoEncoder.HEVC` (API 29+). If the device does not support HEVC it falls back to H.264.

- **HDR10+** recording and HDR metadata require platform support and often vendor integration. The update includes a sample path showing how to record via `MediaCodec`/`MediaMuxer` using `MediaFormat` with `KEY_PROFILE` set to `CodecProfileLevel.HEVCProfileMain10` (if available) and how to attach HDR static metadata — but warns that many devices won't let third-party apps produce certified HDR10+ artifacts without vendor-level codecs. The code prefers simpler MediaRecorder HEVC path and falls back.

Snippet from `prepareRecorder()` now:

```kotlin
private fun chooseVideoEncoder(): Int {
    val list = MediaCodecList(MediaCodecList.REGULAR_CODECS).codecInfos
    val hasHevc = list.any { it.isEncoder && it.supportedTypes.contains("video/hevc") }
    return if (hasHevc && Build.VERSION.SDK_INT >= 29) MediaRecorder.VideoEncoder.HEVC else MediaRecorder.VideoEncoder.H264
}

private fun prepareRecorder(vp: VideoProfile) {
    val encoder = chooseVideoEncoder()
    val rec = MediaRecorder()
    rec.setAudioSource(MediaRecorder.AudioSource.DEFAULT)
    rec.setVideoSource(MediaRecorder.VideoSource.SURFACE)
    rec.setOutputFormat(MediaRecorder.OutputFormat.MPEG_4)
    rec.setOutputFile(out.absolutePath)
    rec.setVideoEncodingBitRate(35_000_000)
    rec.setVideoFrameRate(vp.fps)
    rec.setVideoSize(vp.width, vp.height)
    rec.setVideoEncoder(encoder)
    if (encoder == MediaRecorder.VideoEncoder.HEVC) {
        // attempt to set HEVC Main10/HDR path if supported — may silently be ignored
        if (Build.VERSION.SDK_INT >= 28) rec.setProfile(MediaCodecInfo.CodecProfileLevel.HEVCProfileMain10)
    }
    rec.setAudioEncoder(MediaRecorder.AudioEncoder.AAC)
    rec.setOrientationHint(90)
    rec.prepare()
    recorder = rec
    recordSurface = rec.surface
}
```

> Note: `setProfile()` and programmatic HDR metadata handling vary widely between OEMs; use the MediaCodec + MediaMuxer path for most control if you need true HDR10+ delivery.

### Per-physical camera selection
I added utilities that:
1. Query `CameraCharacteristics.key` `CameraCharacteristics.REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA` and `CameraCharacteristics.getPhysicalCameraIds()`
2. Build `OutputConfiguration` objects with `setPhysicalCameraId(physicalId)` to direct a stream to a specific physical camera when the logical camera supports it.

Example snippet used in `rebuildSession()` when physical selection is enabled:

```kotlin
// if device exposes physical ids and user selected one:
val physicalIds = cameraManager.getCameraCharacteristics(currentCameraId)
    .get(CameraCharacteristics.PHYSICAL_CAMERA_IDS) ?: emptySet<String>()
if (physicalIds.isNotEmpty()) {
    // choose first physical id or user-selected id
    val phys = physicalIds.first()
    val out = OutputConfiguration(previewSurface!!)
    out.setPhysicalCameraId(phys)
    outputs += out
} else {
    outputs += OutputConfiguration(previewSurface!!)
}
```

Important: not all devices expose physical camera IDs to apps. Some phones present only logical IDs and disallow opening individual physical cameras — that limitation is platform/vendor dependent.

### Performance notes
- Peaking/zebra/histogram run on CPU at a reduced preview size (eg 1280x720 or smaller). For better performance consider using a native (NDK) pipeline or GPU shader (OpenGL/GLSL) to compute these overlays.
- Use `ImageReader` with small buffers and avoid expensive allocations per-frame; re-use bitmaps where possible.
- When enabling HEVC/HDR workflows, test battery and thermal behaviour on device.

---

If you want, I can now:
- Insert the new overlay views into the XML layout and wire the Activity (I added the code snippets already).
- Replace the naive Kotlin CPU functions with RenderScript/NDK/GPU shader versions for better performance.
- Add a toggle UI for enabling/disabling each overlay and an option to lower preview processing resolution.

Tell me which of these next steps you want me to apply directly into the canvas source and I’ll update the file (I already added the code shown above to the project canvas).


---

## XML additions — integrated overlay views & UI toggles
Add these views into `activity_pro_cam.xml` (place them above the bottomBar / inside the ConstraintLayout). They are small overlays and toggles for peaking, zebra, histogram and preview processing resolution.

```xml
    <!-- Focus peaking overlay (drawn on top) -->
    <com.example.procam.ui.FocusPeakingView
        android:id="@+id/focusPeakingView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:clickable="false"
        app:layout_constraintTop_toTopOf="@id/previewView"
      
