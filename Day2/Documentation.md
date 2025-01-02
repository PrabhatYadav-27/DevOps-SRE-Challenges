# AWS S3 Bucket Cost Analysis and Management: Documentation

## Objective

The objective of this project was to analyze AWS S3 bucket usage, calculate associated costs, and generate actionable insights regarding unused or underutilized buckets. This analysis is important for cost optimization and resource management in cloud environments. The task involves:
- **Task 1**: Generating a summary of S3 buckets’ metadata such as name, region, size, and versioning.
- **Task 2**: Identifying unused buckets.
- **Task 3**: Generating a cost report based on storage size for each region and team.
- **Task 4**: Providing actions (deletion or archiving) for resource optimization.

## Problem Statement

AWS S3 storage costs can be unpredictable, especially when dealing with numerous buckets. The goals for this task were:
- **Task 1**: Summarize the S3 buckets’ metadata such as name, region, size, and versioning.
- **Task 2**: Identify unused buckets (not accessed for over 90 days).
- **Task 3**: Generate a cost report based on storage size for each region and team.
- **Task 4**: Recommend actions based on usage patterns, including deletion of large unused buckets or archiving.

## Approach

1. **JSON Data Loading**:
    - The bucket data was assumed to be in a `buckets.json` file containing bucket metadata, including size, region, versioning status, and access history (last access or creation date).

2. **Cost Calculation**:
    - The cost of storage was calculated for each bucket based on its size and region. Different storage classes were considered (e.g., Standard and Glacier) with respective cost values.

3. **Days Since Last Access Calculation**:
    - A function was created to calculate the number of days since the last access of a bucket. If the `lastAccessed` field was not available, the script fell back to the bucket’s creation date.

4. **Cost Summary**:
    - Total cost was computed for each region and team by multiplying the storage size of buckets by the cost per GB (Standard and Glacier).

5. **Identification of Unused Buckets**:
    - Buckets that had not been accessed for over 90 days were flagged as unused.

6. **Action Recommendations**:
    - Based on the bucket size and last access date, buckets were recommended for deletion if:
        - They were larger than 100GB and not accessed for more than 20 days.
    - Buckets were recommended for archiving if:
        - They had not been accessed for over 90 days.

7. **Data Output**:
    - The code printed reports of the following:
        - A summary of each bucket's metadata.
        - A cost breakdown by region and team.
        - A list of buckets that are candidates for deletion or archiving.

