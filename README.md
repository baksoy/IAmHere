##Quick explanation of methods used:

This is based on the following [blog post](http://blog.teamtreehouse.com/beginners-guide-location-android)

<pre style="background:#fdf6e3;color:#586e75"><span style="color:#073642;font-weight:700">protected</span> <span style="color:#073642;font-weight:700">void</span> onCreate(<span style="color:#073642;font-weight:700">Bundle</span> savedInstanceState){
        setContentView(<span style="color:#073642;font-weight:700">R</span><span style="color:#859900">.</span>layout<span style="color:#859900">.</span>activity_maps);
        setUpMapIfNeeded();

        mGoogleApiClient<span style="color:#859900">=</span><span style="color:#859900">new</span> <span style="color:#073642;font-weight:700">GoogleApiClient</span>.<span style="color:#073642;font-weight:700">Builder</span>(<span style="color:#268bd2">this</span>)
        .addConnectionCallbacks(<span style="color:#268bd2">this</span>)
        .addOnConnectionFailedListener(<span style="color:#268bd2">this</span>)
        .addApi(<span style="color:#073642;font-weight:700">LocationServices</span><span style="color:#cb4b16"><span style="color:#859900">.</span>API</span>)
        .build();

        <span style="color:#93a1a1">//Setting a data object that contains quality of service parameters for requests </span>
        mLocationRequest<span style="color:#859900">=</span><span style="color:#073642;font-weight:700">LocationRequest</span><span style="color:#859900">.</span>create()
        .setPriority(<span style="color:#073642;font-weight:700">LocationRequest</span><span style="color:#cb4b16"><span style="color:#859900">.</span>PRIORITY_HIGH_ACCURACY</span>)
        .setInterval(<span style="color:#d33682">10</span><span style="color:#859900">*</span><span style="color:#d33682">1000</span>)         <span style="color:#93a1a1">// 10 seconds, in milliseconds</span>
        .setFastestInterval(<span style="color:#d33682">1</span><span style="color:#859900">*</span><span style="color:#d33682">1000</span>);
        }

<span style="color:#073642;font-weight:700">protected</span> <span style="color:#073642;font-weight:700">void</span> onResume(){
        setUpMapIfNeeded();
        mGoogleApiClient<span style="color:#859900">.</span>connect();
        }

<span style="color:#073642;font-weight:700">protected</span> <span style="color:#073642;font-weight:700">void</span> onPause(){
        <span style="color:#93a1a1">// If connected at onResume, then at onPause stop the location updates and disconnect</span>
        <span style="color:#859900">if</span>(mGoogleApiClient<span style="color:#859900">.</span>isConnected()){
        <span style="color:#073642;font-weight:700">LocationServices</span><span style="color:#859900">.</span><span style="color:#073642;font-weight:700">FusedLocationApi</span><span style="color:#859900">.</span>removeLocationUpdates(mGoogleApiClient,<span style="color:#268bd2">this</span>);
        mGoogleApiClient<span style="color:#859900">.</span>disconnect();
        }
        }
<span style="color:#073642;font-weight:700">private</span> <span style="color:#073642;font-weight:700">void</span> setUpMapIfNeeded(){
        <span style="color:#93a1a1">// Do a null check to confirm that we have not already instantiated the map.</span>
        <span style="color:#859900">if</span>(mMap<span style="color:#859900">==</span><span style="color:#b58900">null</span>){
        <span style="color:#93a1a1">// Try to obtain the map from the SupportMapFragment.</span>
        mMap<span style="color:#859900">=</span>((<span style="color:#073642;font-weight:700">SupportMapFragment</span>)getSupportFragmentManager()<span style="color:#859900">.</span>findFragmentById(<span style="color:#073642;font-weight:700">R</span><span style="color:#859900">.</span>id<span style="color:#859900">.</span>map))
        .getMap();
        <span style="color:#93a1a1">// Check if we were successful in obtaining the map.</span>
        <span style="color:#859900">if</span>(mMap<span style="color:#859900">!=</span><span style="color:#b58900">null</span>){
        setUpMap();
        }
        }
        }

<span style="color:#073642;font-weight:700">private</span> <span style="color:#073642;font-weight:700">void</span> setUpMap(){
<span style="color:#93a1a1">//        You can setup markers based on lat and long here as below:</span>
<span style="color:#93a1a1">//        mMap.addMarker(new MarkerOptions().position(new LatLng(0, 0)).title("Marker"));</span>
        }

<span style="color:#073642;font-weight:700">public</span> <span style="color:#073642;font-weight:700">void</span> onConnected(<span style="color:#073642;font-weight:700">Bundle</span> bundle){
        <span style="color:#93a1a1">// Connection callback. After achieving a connected state by calling mGoogleApiClient.connect(), do the following: </span>
        <span style="color:#073642;font-weight:700">Location</span> location<span style="color:#859900">=</span><span style="color:#073642;font-weight:700">LocationServices</span><span style="color:#859900">.</span><span style="color:#073642;font-weight:700">FusedLocationApi</span><span style="color:#859900">.</span>getLastLocation(mGoogleApiClient);

        <span style="color:#859900">if</span>(location<span style="color:#859900">==</span><span style="color:#b58900">null</span>){
        <span style="color:#93a1a1">// Requests location updates if location does not have a value, while being connected to mGoogleApiClient,</span>
        <span style="color:#93a1a1">// passing in the registered mLocationRequest at onCreate, with this (MainActivity) being the Listener</span>
        <span style="color:#073642;font-weight:700">LocationServices</span><span style="color:#859900">.</span><span style="color:#073642;font-weight:700">FusedLocationApi</span><span style="color:#859900">.</span>requestLocationUpdates(mGoogleApiClient,mLocationRequest,<span style="color:#268bd2">this</span>);

        }<span style="color:#859900">else</span>{
        <span style="color:#93a1a1">// If location does have a value, pass it in the handleNewLocation method and call it</span>
        handleNewLocation(location);
        }
        }

<span style="color:#073642;font-weight:700">private</span> <span style="color:#073642;font-weight:700">void</span> handleNewLocation(<span style="color:#073642;font-weight:700">Location</span> location){
        <span style="color:#073642;font-weight:700">double</span> currentLatitude<span style="color:#859900">=</span>location<span style="color:#859900">.</span>getLatitude();
        <span style="color:#073642;font-weight:700">double</span> currentLongitude<span style="color:#859900">=</span>location<span style="color:#859900">.</span>getLongitude();
        <span style="color:#073642;font-weight:700">LatLng</span> latLng<span style="color:#859900">=</span><span style="color:#859900">new</span> <span style="color:#073642;font-weight:700">LatLng</span>(currentLatitude,currentLongitude);

        <span style="color:#073642;font-weight:700">MarkerOptions</span> options<span style="color:#859900">=</span><span style="color:#859900">new</span> <span style="color:#073642;font-weight:700">MarkerOptions</span>()
        .position(latLng)
        .title(<span style="color:#269186"><span style="color:#c60000">"</span>I am here!<span style="color:#c60000">"</span></span>);
        mMap<span style="color:#859900">.</span>addMarker(options);
        <span style="color:#93a1a1">//Focus and Zoom in to current lat and long</span>
        mMap<span style="color:#859900">.</span>moveCamera(<span style="color:#073642;font-weight:700">CameraUpdateFactory</span><span style="color:#859900">.</span>newLatLngZoom(latLng,<span style="color:#d33682">12</span>));
        }

<span style="color:#93a1a1">//Called when the client is temporarily in a disconnected state</span>
<span style="color:#073642;font-weight:700">public</span> <span style="color:#073642;font-weight:700">void</span> onConnectionSuspended(<span style="color:#073642;font-weight:700">int</span> i){
        }

<span style="color:#93a1a1">// Called when there was an error connecting the client to the service</span>
<span style="color:#073642;font-weight:700">public</span> <span style="color:#073642;font-weight:700">void</span> onConnectionFailed(<span style="color:#073642;font-weight:700">ConnectionResult</span> connectionResult){
        <span style="color:#859900">if</span>(connectionResult<span style="color:#859900">.</span>hasResolution()){
        <span style="color:#859900">try</span>{
        <span style="color:#93a1a1">// Start an Activity that tries to resolve the error</span>
        connectionResult<span style="color:#859900">.</span>startResolutionForResult(<span style="color:#268bd2">this</span>,<span style="color:#cb4b16">CONNECTION_FAILURE_RESOLUTION_REQUEST</span>);
        }<span style="color:#859900">catch</span>(<span style="color:#073642;font-weight:700">IntentSender</span><span style="color:#859900">.</span><span style="color:#073642;font-weight:700">SendIntentException</span> e){
        e<span style="color:#859900">.</span>printStackTrace();
        }
        }<span style="color:#859900">else</span>{
        <span style="color:#073642;font-weight:700">Log</span><span style="color:#859900">.</span>i(<span style="color:#cb4b16">TAG</span>,<span style="color:#269186"><span style="color:#c60000">"</span>Location services connection failed with code <span style="color:#c60000">"</span></span><span style="color:#859900">+</span>connectionResult<span style="color:#859900">.</span>getErrorCode());
        }
        }

<span style="color:#073642;font-weight:700">public</span> <span style="color:#073642;font-weight:700">void</span> onLocationChanged(<span style="color:#073642;font-weight:700">Location</span> location){
        <span style="color:#93a1a1">// Call the handleNewLocation again when the location changes</span>
        handleNewLocation(location);
        }
</pre>