# Media Formats

The Elacity Media Player supports industry-standard media formats with various codec options. This document outlines supported formats, codecs, and encoding guidelines.

## Container Format

### MPEG-DASH

The player exclusively supports **MPEG-DASH** (Dynamic Adaptive Streaming over HTTP) format.

- **Manifest**: `.mpd` (Media Presentation Description) file
- **Segments**: Fragmented MP4 segments (`.m4s` files)
- **Encryption**: Common Encryption (CENC) with `cenc` or `cbcs` schemes

## Video Codecs

### H.264/AVC ✅
- **MediaSource Support**: ✅ Full support
- **Encoding Support**: ✅ Full support
- **Encoders**: `libx264`, `h264_vaapi`, `h264_v4l2m2m`
- **Profiles**: Baseline, Main, High
- **Best For**: Maximum compatibility

### AV1 ✅
- **MediaSource Support**: ✅ Full support
- **Encoding Support**: ✅ Full support
- **Encoders**: `libaom-av1`, `libdav1d`
- **Best For**: Modern browsers, better compression

### VP9 ✅
- **MediaSource Support**: ✅ supported
- **Encoding Support**: ✅ Supported
- **Encoders**: `vp9_vaapi`
- **Note**: Can be decoded but not played via MSE

### H.265/HEVC ❌
- **MediaSource Support**: ❌ Not supported
- **Encoding Support**: ❌ Not supported
- **Note**: Not recommended

## Audio Codecs

### AAC ✅
- **MediaSource Support**: ✅ Full support
- **Encoding Support**: ✅ Full support
- **Best For**: Maximum compatibility

### Opus ✅
- **MediaSource Support**: ✅ Full support
- **Encoding Support**: ✅ Full support
- **Best For**: High quality audio

### Vorbis ❓
- **MediaSource Support**: ❓ Unknown
- **Encoding Support**: ❓ Unknown
- **Note**: Not commonly used

## Encoding Guidelines

### Multi-Bitrate Encoding

For adaptive streaming, encode multiple bitrates:

```bash
# 2160p (4K)
ffmpeg -i input.mp4 \
  -c:v h264 -profile:v high -level:v 5.1 \
  -maxrate 30000k -bufsize 60000k \
  -r 25 -g 50 -sc_threshold 0 \
  -force_key_frames "expr:gte(t,n_forced*2)" \
  -c:a aac -pix_fmt yuv420p \
  -movflags +faststart \
  -vf "scale=3840:-2" 2160.mp4

# 1080p
ffmpeg -i input.mp4 \
  -c:v h264 -profile:v main -level:v 4.0 \
  -maxrate 8000k -bufsize 16000k \
  -r 25 -g 50 -sc_threshold 0 \
  -force_key_frames "expr:gte(t,n_forced*2)" \
  -c:a aac -pix_fmt yuv420p \
  -movflags +faststart \
  -vf "scale=1920:-2" 1080.mp4

# 720p
ffmpeg -i input.mp4 \
  -c:v h264 -profile:v main -level:v 4.0 \
  -maxrate 3000k -bufsize 6000k \
  -r 25 -g 50 -sc_threshold 0 \
  -force_key_frames "expr:gte(t,n_forced*2)" \
  -c:a aac -pix_fmt yuv420p \
  -movflags +faststart \
  -vf "scale=1280:-2" 720.mp4
```

### DASH Fragmentation

After encoding, fragment and package for DASH:

```bash
# Fragment all bitrates
for res in 2160 1440 1080 720 480 360 240 144; do
  mp4fragment --fragment-duration 2 "${res}.mp4" "output/${res}_fragmented.mp4"
done

# Generate DASH manifest
mp4dash \
  --output-dir=dash \
  --use-segment-timeline \
  output/2160_fragmented.mp4 \
  output/1440_fragmented.mp4 \
  output/1080_fragmented.mp4 \
  output/720_fragmented.mp4 \
  output/480_fragmented.mp4 \
  output/360_fragmented.mp4 \
  output/240_fragmented.mp4 \
  output/144_fragmented.mp4
```

### Encryption

Encrypt segments with Common Encryption:

```bash
# Encrypt with CENC
mp4encrypt \
  --method MPEG-CENC \
  --key 1:YOUR_KEY_ID:YOUR_KEY \
  --property 1:KID:YOUR_KEY_ID \
  input.mp4 output_encrypted.mp4
```

## Codec Selection Guide

### Maximum Compatibility
- **Video**: H.264 High Profile
- **Audio**: AAC
- **Works On**: All supported browsers

### Best Quality
- **Video**: AV1 (desktop) or H.264 (mobile)
- **Audio**: Opus (desktop) or AAC (mobile)
- **Works On**: Modern browsers

### Smallest File Size
- **Video**: AV1
- **Audio**: Opus
- **Works On**: Chrome, Firefox, Edge (desktop)

## Adaptive Streaming

The player supports adaptive bitrate streaming (ABR) with the BOLA algorithm (planned for v0.3.0).

### Current Behavior
- Always selects first available stream
- No automatic quality switching

### Planned Behavior (v0.3.0)
- Buffer-based quality selection
- Bandwidth-aware initial segment selection
- Dynamic quality switching

## Format Requirements

### DASH Manifest Requirements

```xml
<?xml version="1.0" encoding="UTF-8"?>
<MPD xmlns="urn:mpeg:dash:schema:mpd:2011" type="static" mediaPresentationDuration="PT10M">
  <Period>
    <AdaptationSet mimeType="video/mp4" segmentAlignment="true">
      <Representation id="1" bandwidth="8000000" width="1920" height="1080">
        <BaseURL>1080p/</BaseURL>
        <SegmentTemplate timescale="1000" duration="2000" 
                         initialization="init.mp4" 
                         media="seg_$Number$.m4s"/>
      </Representation>
    </AdaptationSet>
  </Period>
</MPD>
```

### PSSH Box Requirements

The manifest must include PSSH (Protection System Specific Header) boxes:

```xml
<ContentProtection schemeIdUri="urn:mpeg:dash:mp4protection:2011" 
                  value="cenc">
  <cenc:pssh>BASE64_ENCODED_PSSH</cenc:pssh>
</ContentProtection>
```

## Browser Compatibility Matrix

| Codec | Chrome | Firefox | Safari | Edge |
|-------|--------|---------|--------|------|
| H.264 | ✅ | ✅ | ✅ | ✅ |
| AV1 | ✅ | ✅ | ❌ | ✅ |
| VP9 | ✅ | ✅ | ❌ | ✅ |
| AAC | ✅ | ✅ | ✅ | ✅ |
| Opus | ✅ | ✅ | ⚠️ | ✅ |

## Related Documentation

- [Architecture Overview](overview.md) - Player architecture
- [Player API](../api/player.md) - API reference
- [Troubleshooting](../development/troubleshooting.md) - Format-related issues
