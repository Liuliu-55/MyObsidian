# CARRADA Dasets

```shell
| - annotation_generator
| | - centroid_tracking.py
| | | - CentroidMatching
| | | | - _init_
| | | | - _create_clusters
| | | | - _get_mean_shift
| | | | - select_bandwidth
| | | | - get_doa_points
| | | | - get_doa_lables
| | | | - get_centroids
| | | | - get_cluster_size
| | | | - get_cluster
| | | | - update
| | | - CentroidTracking
| | | | - _init_
| | | | - _get_paths
| | | | - select_doa_points
| | | | - _get_ref_coord
| | | | - _process_sequence
| | | | - _track
| | | | - create_annotations
| | | | - _save_annotations
| | - instance_generator.py
| | | - InstanceGenetaroter
| | | | - _init_
| | | | - _get_paths
| | | | - _process_images
| | | | - _save_data
| | | | - _set_points
| | | | - process_sequence
| | | | - set_paths
| | | | - get_points





| | | - ImageInstances
| | | - Instance
| | - rd_points_generator.py
| | | - RDPointsGenerator
| | | - InstancePoints
| | | - InstanceRDPoints
| | | - RDPoints
| | - utils.py
| - scripts
| | - generate_annotation_files.py
| | - generate_centrroids.py
| | - generate_instances.py
| | - generate_rd_points.py
| | - run_annotation_pipeline.sh
| | - set_paths.py
| - tests
| | - run_tests.sh
| | - test_annotation_files.py
| | - test_centroids.py
| | - test_instances.py
| | - test_rd_points.py
| | - test_transform_data.py
| - utils
| | - camera.py
| | - configurable.py
| | - generate_annotations.py
| | - sort.py
| | - transform_annotations.py
| | - transform_data.py
| | - config.ini
| | - visualize_signal.py
```


