# Segmentation ROS Message
`segmentation_ros_msg` is a ROS message-only package designed to represent image-based segmentation and instance segmentation results in a compact and efficient form.  

## Package Overview
- **Package name**: `segmentation_ros_msg`
- **Supported ROS**: ROS1, ROS2
- **Purpose**:
    - Transfer segmentation and instance segmentation outputs
    - Support vision-language and prompt-based segmentation models
    - Provide a lightweight and consistent ROS interface  

## Message Definition
`SegmentationResult.msg`  
```
std_msgs/Header header
string prompt
int32 object_num

sensor_msgs/Image label_mask

float32[] boxes
float32[] scores
```  

## Field Descriptions
- `header` (`std_msgs/Header`)  
    - Standard ROS message header
- `prompt` (`std_msgs/String`)  
    - Text prompt used for segmentation  
    - Examples: `"person"`, `"tree"`  
    - This field is intended for vision-language or prompt-conditioned segmentation models (e.g., SAM-based systems).
- `object_num` (`std_msgs/Int32`)  
    - Number of segmented objects `N`  
    - Must be consistent with other fields:
        - `scores.size() == object_num`  
        - `boxes.size() == 4 * object_num`
- `label_mask` (`sensor_msgs/Image`)  
    - Label-based segmentation mask
    - Recommended encoding: `mono16` or `16UC1`
    - Pixel value semantics:
        - `0`: background
        - `1~N`: object IDs
    - This design avoids transmitting N separate binary masks and significantly reduces message size.
- `boxes` (`std_msgs/Float32MultiArray`)  
    - Flattened bounding box array
    - Format:  
        ```
        [x1, y1, x2, y2,  x1, y1, x2, y2, ...]
        ```  
    - Length: `4 * object_num`
    - Coordinate convention:
        - Pixel coordinates in image space
        - `(x1, y1)` = top-left
        - `(x2, y2)` = bottom-right
- `scores` (`std_msgs/Float32MultiArray`)  
    - Confidence score for each segmented object
    - Length: `object_num`
    - Range: `[0.0, 1.0]`  

## Example
Example is used in [this link]()