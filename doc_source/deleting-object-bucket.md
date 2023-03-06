# Step 5: Delete your objects and bucket<a name="deleting-object-bucket"></a>

When you no longer need an object or a bucket, we recommend that you delete them to prevent further charges\. If you completed this getting started walkthrough as a learning exercise, and you don't plan to use your bucket or objects, we recommend that you delete your bucket and objects so that charges no longer accrue\. 

Before you delete your bucket, empty the bucket or delete the objects in the bucket\. After you delete your objects and bucket, they are no longer available\.

If you want to continue to use the same bucket name, we recommend that you delete the objects or empty the bucket, but don't delete the bucket\. After you delete a bucket, the name becomes available to reuse\. However, another AWS account might create a bucket with the same name before you have a chance to reuse it\. 

**Topics**
+ [Deleting an object](#clean-up-delete-objects)
+ [Emptying your bucket](#clean-up-empty-bucket)
+ [Deleting your bucket](#clean-up-delete-bucket)

## Deleting an object<a name="clean-up-delete-objects"></a>

If you want to choose which objects you delete without emptying all the objects from your bucket, you can delete an object\. 

1. In the **Buckets** list, choose the name of the bucket that you want to delete an object from\.

1. Select the object that you want to delete\.

1. Choose **Delete** from the options in the upper right\. 

1. On the **Delete objects** page, type **delete** to confirm deletion of your objects\.

1. Choose **Delete objects**\.

## Emptying your bucket<a name="clean-up-empty-bucket"></a>

If you plan to delete your bucket, you must first empty your bucket, which deletes all the objects in the bucket\. 

**To empty a bucket**



1. In the **Buckets** list, select the bucket that you want to empty, and then choose **Empty**\.

1. To confirm that you want to empty the bucket and delete all the objects in it, in **Empty bucket**, type **permanently delete**\.
**Important**  
Emptying the bucket cannot be undone\. Objects added to the bucket while the empty bucket action is in progress will be deleted\.

1. To empty the bucket and delete all the objects in it, and choose **Empty**\.

   An **Empty bucket: Status** page opens that you can use to review a summary of failed and successful object deletions\.

1. To return to your bucket list, choose **Exit**\.

## Deleting your bucket<a name="clean-up-delete-bucket"></a>

After you empty your bucket or delete all the objects from your bucket, you can delete your bucket\.

1. To delete a bucket, in the **Buckets** list, select the bucket\.

1. Choose **Delete**\.

1. To confirm deletion, in **Delete bucket**, type the name of the bucket\.
**Important**  
Deleting a bucket cannot be undone\. Bucket names are unique\. If you delete your bucket, another AWS user can use the name\. If you want to continue to use the same bucket name, don't delete your bucket\. Instead, empty and keep the bucket\. 

1. To delete your bucket, choose **Delete bucket**\.