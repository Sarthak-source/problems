import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:flip_card/flip_card.dart';
import 'package:flutter/material.dart';
import 'package:geolocator/geolocator.dart';
import 'package:provider/provider.dart';
import 'package:comcrop/models/users.dart';

class FlipCropCard extends StatefulWidget {
  final Widget croptype;
  final Map<String, dynamic> profile;
  final int index;
  FlipCropCard({this.croptype, this.profile, this.index});
  @override
  _FlipCropCardState createState() => _FlipCropCardState();
}

class _FlipCropCardState extends State<FlipCropCard> {
  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: EdgeInsets.all(15),
      child: FlipCard(
        front: Container(
            width: 225,
            height: 500,
            decoration: BoxDecoration(
                boxShadow: [
                  BoxShadow(
                    color: Colors.black26,
                    offset: Offset(0.0, 5.0), //(x,y)
                    blurRadius: 10.0,
                  ),
                ],
                color: Colors.white,
                borderRadius: BorderRadius.all(Radius.circular(10.0))),
            child: Padding(
              padding: EdgeInsets.all(10.0),
              child: Center(
                child: widget.croptype ??
                    Text('no info given',
                        style: Theme.of(context).textTheme.headline2),
              ),
            )),
        back: Container(
            width: 225,
            height: 500,
            decoration: BoxDecoration(
                boxShadow: [
                  BoxShadow(
                    color: Colors.black26,
                    offset: Offset(0.0, 5.0), //(x,y)
                    blurRadius: 10.0,
                  ),
                ],
                color: Colors.white,
                borderRadius: BorderRadius.all(Radius.circular(10.0))),
            child: Padding(
              padding: EdgeInsets.all(10),
              child: Column(
                children: [
                  Card(
                    color: Colors.orange[300],
                    elevation: 5.0,
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(25),
                    ),
                    child: Padding(
                      padding: EdgeInsets.all(10.0),
                      child: Row(
                        children: <Widget>[
                          CircleAvatar(
                            radius: 25,
                            backgroundImage:
                                NetworkImage(widget.profile['profilePic']),
                          ),
                          SizedBox(width: 10),
                          Flexible(
                            child: Text(widget.profile['firstName'],
                                overflow: TextOverflow.ellipsis,
                                style: Theme.of(context)
                                    .textTheme
                                    .headline2
                                    .copyWith(
                                        fontSize: 13.0,
                                        color: Colors.white,
                                        fontWeight: FontWeight.bold)),
                          ),
                          SizedBox(width: 4),
                          Flexible(
                            child: Text(widget.profile['lastName'],
                                overflow: TextOverflow.ellipsis,
                                style: Theme.of(context)
                                    .textTheme
                                    .headline2
                                    .copyWith(
                                      fontSize: 13.0,
                                      color: Colors.white,
                                      fontWeight: FontWeight.bold,
                                    )),
                          )
                        ],
                      ),
                    ),
                  ),
                  SizedBox(height: 5),
                  Expanded(
                    child: Card(
                      color: Colors.white,
                      elevation: 5.0,
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      child: Padding(
                        padding: const EdgeInsets.all(10.0),
                        child: Center(
                          child: Flexible(
                            child: Text(widget.profile['address'],
                                overflow: TextOverflow.ellipsis,
                                textAlign: TextAlign.center,
                                style: Theme.of(context)
                                    .textTheme
                                    .headline2
                                    .copyWith(fontSize: 13.0)),
                          ),
                        ),
                      ),
                    ),
                  ),
                  SizedBox(height: 10),
                  Center(
                    child: Text(
                        '${(widget.profile['distance']).toString()} km away' ??
                            '0',
                        overflow: TextOverflow.ellipsis,
                        style: Theme.of(context)
                            .textTheme
                            .headline2
                            .copyWith(fontSize: 13.0)),
                  ),
                  SizedBox(height: 5),
                  Center(
                    child: Row(
                      children: [
                        Flexible(
                          child: RaisedButton(
                              elevation: 5,
                              child: Row(
                                children: [
                                  Icon(Icons.call, color: Colors.white),
                                  SizedBox(width: 4),
                                  Flexible(
                                    child: Text(
                                        '${widget.profile['firstName']}',
                                        overflow: TextOverflow.ellipsis,
                                        style:
                                            Theme.of(context).textTheme.button),
                                  ),
                                ],
                              ),
                              onPressed: () {
                                print('object');
                              }),
                        ),
                        SizedBox(width: 8),
                        Flexible(
                          child: RaisedButton(
                              elevation: 5,
                              child: Text('view info',
                                  overflow: TextOverflow.ellipsis,
                                  style: Theme.of(context).textTheme.button),
                              onPressed: () {
                                print('object');
                              }),
                        ),
                      ],
                    ),
                  )
                ],
              ),
            )),
      ),
    );
  }
}

class SpecificCropList extends StatefulWidget {
  @override
  _SpecificCropListState createState() => _SpecificCropListState();
}

class _SpecificCropListState extends State<SpecificCropList> {
  @override
  Widget build(BuildContext context) {
    final profiles = Provider.of<List<UserProfileData>>(context) ?? [];

    final user = Provider.of<User>(context);
    var userindex;

    getcurrentuserindex(var uid) async {
      var currentuser = await Firestore.instance
          .collection('profileData')
          .document(uid)
          .get();
      for (var i = 0; i < profiles.length; i++) {
        if (currentuser.documentID == profiles[i].uid) {
          userindex = profiles.indexOf(profiles[i]);
        }
      }
      return userindex;
    }

    distancef(var x, var y) async {
      return await Geolocator().distanceBetween(
          profiles[await getcurrentuserindex(user.uid)].location.longitude,
          profiles[await getcurrentuserindex(user.uid)].location.latitude,
          x,
          y);
    }

    Future<List<Map<String, dynamic>>> sortByDistance(
        List<UserProfileData> profilelist) async {
      List<Map<String, dynamic>> locationListWithDistance = List();

      // associate location with distance
      for (var profile in profilelist) {
        double distance = await distancef(
            profile.location.latitude, profile.location.longitude);
        locationListWithDistance.add({
          'uid': profile.uid,
          'firstName': profile.firstName,
          'lastName': profile.lastName,
          'phoneNumber': profile.phoneNumber,
          'address': profile.address,
          'profilePic': profile.profilePic,
          'distance': distance,
        });
      }

      // sort by distance
      locationListWithDistance.sort((a, b) {
        int d1 = a['distance'];
        int d2 = b['distance'];
        if (d1 > d2)
          return 1;
        else if (d1 < d2)
          return -1;
        else
          return 0;
      });

      return locationListWithDistance;
    }

    return FutureBuilder(
      future: sortByDistance(profiles),
      builder: (context, snapshot) {
        var sorted = snapshot.data as List<Map<String, dynamic>>;
        debugPrint(sorted.toString());

        return ListView.builder(
            scrollDirection: Axis.horizontal,
            itemCount: 2,
            itemBuilder: (context, index) {
              return FlipCropCard(profile: sorted[index]);
            });
      },
    );
  }
}
