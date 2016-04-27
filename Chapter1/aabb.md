#AABB - Axis Aligned Bounding Box

An AABB (Axis Aligned Bounding Box) is a 3D box. It's width / height / depth don't have to be equal, but the width is always aligned to the X axis, the height to the Y axis and depth to the Z axis. That is to say, this box can't be rotated.

AABB's are pivotal to spacial partitioning, they let us cut a section of 3D space up into smaller sections of 3D space. AABB's also serve as a great acceleration structure for hit detection, but more on that later.

