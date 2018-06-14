
## 网易云信，互动白板

```
2018-06-14 17:28:21 - 2018-06-14 18:03:09

流程：
--------------------------------------

1.创建聊天室 - requestChatRoom

2.预约聊天室 - reserveNetCallMeeting

3.进入聊天室 - enterChatRoom

--------------------------------------


详细：

--------------------------------------

1.创建聊天室 - requestChatRoom

// 创建请求
NSString *urlString = @"https://app.netease.im/api/chatroom/create";
NSURL *url = [NSURL URLWithString:urlString];
NSMutableURLRequest *request = [[NSMutableURLRequest alloc] initWithURL:url
                                                            cachePolicy:NSURLRequestUseProtocolCachePolicy
                                                        timeoutInterval:30];
[request setHTTPMethod:@"POST"];
[request addValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
[request addValue:@"appKey 的值" forHTTPHeaderField:@"appKey"];
NSData *data = ({
    
    NSMutableDictionary *dic = @{}.mutableCopy;
    NSString *currentUserId = [[NIMSDK sharedSDK].loginManager currentAccount];
    NSDictionary *ext = @{@"meeting":@"meetingName 的值"};
    NSData *extData = [NSJSONSerialization dataWithJSONObject:ext options:0 error:nil];
    NSString *extString = [[NSString alloc] initWithData:extData
                                                encoding:NSUTF8StringEncoding];
    
    [dic setObject:currentUserId forKey:@"creator"];
    [dic setObject:@"meetingName 的值" forKey:@"name"];
    [dic setObject:extString forKey:@"ext"];
    
    [NSJSONSerialization dataWithJSONObject:dict options:0 error:nil];
})
[request setHTTPBody:data];

// 发起请求
[[[NSURLSession sharedSession] dataTaskWithRequest:request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
    if (error == nil &&
        [response isKindOfClass:[NSHTTPURLResponse class]] &&
        [(NSHTTPURLResponse *)response statusCode] == 200)
    {
        NSDictionary *dic = [NSJSONSerialization JSONObjectWithData:data
                                                            options:0
                                                              error:&error];
        NSLog(@"dic:%@",dic);
        
        // 拿到 meetingRoomID, 再预约聊天室
        
    }
}] resume];


--------------------------------------


#import <NIMAVChat/NIMAVChat.h>
2.预约聊天室 - reserveNetCallMeeting

// 多人音视频会议
NIMNetCallMeeting *meeting = [[NIMNetCallMeeting alloc] init];
meeting.name = // roomId; // 会议名称
meeting.type = NIMNetCallTypeVideo; // 加入会议的音视频类型
meeting.ext = @"test extend meeting messge"; // 扩展信息

// 预订多人会议
[[NIMAVChatSDK sharedSDK].netCallManager reserveMeeting:meeting completion:^(NIMNetCallMeeting *meeting, NSError *error) {
    if (!error) {
        // [self enterChatRoom:roomId]; // 进入聊天室
    }
    else {
        // 预订多人会议失败
    }
}];


--------------------------------------


#import <NIMSDK/NIMSDK.h>
3.进入聊天室 - enterChatRoom

// 进入聊天室请求
NIMChatroomEnterRequest *request = [[NIMChatroomEnterRequest alloc] init];
request.roomId = roomId;

// 进入聊天室
__weak typeof(self) weakself = self;
[[NIMSDK sharedSDK].chatroomManager enterChatroom:request completion:^(NSError *error, NIMChatroom *room, NIMChatroomMember *me) {
    if (!error) {
        // 跳转到聊天室
    }
    else
    {
        // 进入聊天室失败
    }
}];
```
