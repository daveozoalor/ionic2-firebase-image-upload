# ionic2-firebase-image-upload
A sample method to upload image with ionic2 and firebase


````

addPicture(path : any, photoData : any) {

console.log("Photo Data : "+photoData);
	var postDataDetails = {
		question_description : this.question_description,
		question_title : this.question_title,
		right_answer : this.right_answer
	}
  /*
	  let toast = this.toastCtrl.create({
      message: 'Image upload started in the background',
      duration: 3000,
      position: 'top'
    });
    toast.present();
 */

/*
      let loader = this.loadingCtrl.create({
           content: "Uploading Image, Please Wait...",
         });
         loader.present();

  if (photoData != null) {


  	var that = this;
     var newDetailsKey = firebase.database().ref().child(path).push(postDataDetails);

        that.profilePictureRef.child(that.categoryId+'/questions/'+newDetailsKey.key).child('gamePicture.png')
      .putString(photoData, 'base64', {contentType: 'image/png'})
        .then(savedPicture => {


        // Get task progress, including the number of bytes uploaded and the total number of bytes to be uploaded
     that.uploadProgress = (savedPicture.bytesTransferred / savedPicture.totalBytes) * 100;


    if(that.uploadProgress == 100){
      loader.dismiss();
    }


            	//update the question with photo url
             firebase.database().ref().child(path+'/'+newDetailsKey.key)
              .update({photoUrl : savedPicture.downloadURL});


      // console.log('Upload is ' + that.uploadProgress + '% done');

       let toast = that.toastCtrl.create({
         message: 'Upload is ' + that.uploadProgress + '% done',
         duration: 3000,
         position: 'top'
       });
       toast.present();

      return  that.uploadProgress;

        });
      }



*/



  // Create the file metadata
  var metadata = {
    contentType: 'image/jpeg'
  };
 var file = photoData;
 var that = this;

 var loader = that.loadingCtrl.create({
      content: "Uploading Image, "+ that.progress +" Please Wait...",
    });


  // Upload file and metadata to the object 'images/mountains.jpg'
  var uploadTask = that.profilePictureRef.child('images/' + 'imageUploadPicture.png').putString(photoData);

  // Listen for state changes, errors, and completion of the upload.
  uploadTask.on(firebase.storage.TaskEvent.STATE_CHANGED, function(snapshot) {
      // Get task progress, including the number of bytes uploaded and the total number of bytes to be uploaded
      that.progress = (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
      console.log('Upload is ' + that.progress + '% done');


         loader.present();

      switch (snapshot.state) {
        case firebase.storage.TaskState.PAUSED: // or 'paused'
          console.log('Upload is paused');
          break;
        case firebase.storage.TaskState.RUNNING: // or 'running'
          console.log('Upload is running');
          break;
      }
    }, error => {
    switch (error.code) {
      case 'storage/unauthorized':
        // User doesn't have permission to access the object
        break;

      case 'storage/canceled':
        // User canceled the upload
        break;
      case 'storage/unknown':
        // Unknown error occurred, inspect error.serverResponse
        break;
    }
  }, savedPicture =>{
    // Upload completed successfully, now we can get the download URL

     loader.dismiss();
    var downloadURL = uploadTask.snapshot.downloadURL;

    var postDataDetails = {
  		question_description : that.question_description,
  		question_title : that.question_title,
  		right_answer : that.right_answer
  	}
     var newDetailsKey = firebase.database().ref().child(path).push(postDataDetails);
    //update the question with photo url
   firebase.database().ref().child(path+'/'+newDetailsKey.key)
    .update({photoUrl : savedPicture.downloadURL});
  });





  }

````
