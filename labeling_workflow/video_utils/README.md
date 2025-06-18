# Video Utils (Splitting and Stitching) Pipeline

Pipeline Template for **Video Split and Stitch Operations**, providing a streamlined workflow for video processing

<img src="assets\video_utils_pipeline.png">

### Pipeline Flow Details:

1. **Video to Videos Node**
   - Split Type: "split by number of frames per sub video"
   - Split Argument: 500 (creates sub-videos of 500 frames each) 
   - Purpose: Splits long videos into shorter segments to avoid sending more than 1000 frames to downstream nodes
   - Output: Multiple videos, each containing 500 frames, ensuring downstream nodes never receive more than 1000 frames

2. **Videos to Frames Node**
   - Split Type: "split by Frame interval" 
   - Split Argument: 1 (splits video into individual frames)
   - Output Directory: Set your desired output path

3. **Predict Node**
   - Adds bounding boxes and annotations to each frame
   - Uses selected model for detection/classification

4. **Review Task**
   - QA task to review the output of the prediction
   - Manual validation of annotations
   - Allows correction of any prediction errors

5. **Wait Node**
   - Ensures all previous tasks are completed
   - Synchronization point before merging

6. **Merge Node**
   - Dataset Input Directory: Must match Split Node output directory
   - FPS: Set to match original video's FPS
   - Tracker Configuration:
     - Select either ByteTrack or DeepSORT
     - Adjust tracking parameters as needed:
       - min_box_area: Minimum bounding box area (in pixels) to track. Smaller boxes are filtered out. Higher values reduce false positives but may miss small objects.
       - track_thresh (ByteTrack only): Detection confidence threshold for creating new tracks. Higher values (>0.5) mean more conservative tracking with fewer false tracks.
       - track_buffer: Number of frames to keep track history. Larger buffers help maintain identity through occlusions but increase memory usage.
       - match_thresh: IoU threshold for matching detections to existing tracks. Higher values (>0.8) require more precise matches, lower values allow more flexible matching but may cause ID switches.

**Important**: The output directory of `Video to Videos Node` and `Videos to Frames Node` should be different to avoid collisions. However, the input directory of the `Merge Node` should be the same as the output directory of `Videos to Frames Node`.

**Note**: The merge node automatically filters items based on metadata like `origin_video_name` and `time`, so there's no need to change the working directory between pipeline cycles. Each merge operation will only process videos from the same split/batch.
