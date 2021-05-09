var AWS = require('aws-sdk');
var DateToday = new Date();
var date = DateToday.toISOString();
var timestamp = date.substr(0, 10);
console.log('Today: ' , timestamp);

var DatePrevious = new Date();
DatePrevious.setDate(DatePrevious.getDate() - 30)
var dateprev = DatePrevious.toISOString();
var timestamp_prev = dateprev.substr(0, 10);
console.log('<br>30 days ago was: ' , timestamp_prev);


var INSTANCE_SETTINGS = [
	{
		id: 'i-xxxxxxxxxxxxxxx',
		name: '$servername',
		region: '$regioname'
	}
	
];

exports.handler = function (event, context) {	
	for (var index = 0; index < INSTANCE_SETTINGS.length; index++) {
		let currentIndex = index;
		var ec2 = new AWS.EC2({
			apiVersion: '2016-11-15',
			region: INSTANCE_SETTINGS[index].region,
		});

       var ImageNameString = "auto-ami-" + (INSTANCE_SETTINGS[index].name) + "-" + timestamp;
		var params = {
			Description: "Created from Lambda",
			InstanceId: INSTANCE_SETTINGS[index].id,
			Name: ImageNameString,
			NoReboot: true,
		};
		ec2.createImage(params, function (err, data0) {
			if (err) console.log(err, err.stack);
			else console.log(data0);
		});

        var ImageNameToDelete = "auto-ami-" + (INSTANCE_SETTINGS[index].name) + "-" + timestamp_prev;
        console.log("Image name to delete : ",ImageNameToDelete)
        let params_todelete = {}
        params_todelete.Filters = [...PARAMS_TO_DELETE.Filters]
        params_todelete.Owners = ['self'];
        params_todelete.Filters[0].Values = [ImageNameToDelete];


		ec2.describeImages(params_todelete, function (err, data) {
			if (err) console.log(err, err.stack);
			else {
                if(data.Images.length > 0) {
                    var newec2 = new AWS.EC2({
						apiVersion: '2016-11-15',
						region: INSTANCE_SETTINGS[currentIndex].region,
					});
    
                    newec2.deregisterImage({ ImageId : data.Images[0].ImageId }, function(err, data) {
                        if (err) console.log(err, err.stack); // an error occurred
                        else     console.log("SUCCESS");           // successful response
                    });
                }
            }
        });
	}
};

const PARAMS_TO_DELETE = {
    Filters: [
        {
            Name: 'name',
            Values: [],
        }
    ],
    Owners: ['self']
};
