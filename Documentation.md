Documentation
-------------------------

###### Quick Setup

1) Download or Clone the code.

2) Build the MultipeerKit framework

3) Locate the built MultipeerKit file and add it to your project. Lastly, link the framework in the build setting and 
included the header file MultipeerKit in your project prefix. 

###### Quick Start

1) Make sure you import the MultipeerKit header files.

<pre>
#import MultipeerKit.h
</pre>

2) Include the instance for the delegate and transceiver for the view controller. This could be placed in the header or main of your view controller. 

<pre>

NSString *const kConnectionsContainerViewSegue = @"connectionsContainerViewSegue";
NSString *const kInfoContainerViewSegue = @"infoContainerViewSegue";

@interface MPKViewController ()<MCTransceiverDelegate, MPKPeerInfoViewDelegate>

@property (strong, nonatomic, readonly) MCTransceiver *transceiver;
@property (nonatomic, readonly) MCTransceiverMode mode;
@property (nonatomic) BOOL didFindPeer;

@end

</pre>

3) Before starting the transceiver we have to set it up and pass in parameters. Todo that do the following:

<pre>
-(void)configureTransceiver
{
    _mode = MCTransceiverModeBrowser;
    _transceiver = [[MCTransceiver alloc] initWithDelegate:self
                                                  peerName:[UIDevice currentDevice].name
                                                      mode:self.mode];
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(5 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        if (self.transceiver.connectedPeers.count == 0 && !self.didFindPeer) {
            NSLog(@"no peers found; becoming host (switching to advertising mode)");
            [self.transceiver stopBrowsing];
            [self becomeHost];
        }
    });
}
</pre>

4) Start the transceiver in viewDidload function

<pre>
-(void)viewDidLoad
{
    [super viewDidLoad];

    [self configureTransceiver];
}
</pre>

###### Delegate Methods

<pre>
-(void)didFindPeer:(MCPeerID *)peerID
</pre>
<pre>
-(void)didLosePeer:(MCPeerID *)peerID
</pre>
<pre>
-(void)didReceiveInvitationFromPeer:(MCPeerID *)peerID
</pre>
<pre>
-(void)didConnectToPeer:(MCPeerID *)peerID
</pre>
<pre>
-(void)didDisconnectFromPeer:(MCPeerID *)peerID
</pre>
<pre>
-(void)didReceiveData:(NSData *)data fromPeer:(MCPeerID *)peerID
</pre>
