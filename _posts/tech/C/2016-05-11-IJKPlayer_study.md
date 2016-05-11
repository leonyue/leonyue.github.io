---
layout: post
title: IJKPlayer学习笔记
category: 技术
tags: IJKPlayer
keywords: IJKPlayer
description:
---

##  IjkMediaPlayer *ijkmp_ios_create(int (*msg_loop)(void*))
---

````swift
// init player  
_mediaPlayer = ijkmp_ios_create(media_player_msg_loop);
````

````swift
int media_player_msg_loop(void* arg)
{
    @autoreleasepool {
        IjkMediaPlayer *mp = (IjkMediaPlayer*)arg;
        __weak IJKFFMoviePlayerController *ffpController = ffplayerRetain(ijkmp_set_weak_thiz(mp, NULL));

        while (ffpController) {
            @autoreleasepool {
                IJKFFMoviePlayerMessage *msg = [ffpController obtainMessage];
                if (!msg)
                    break;

                int retval = ijkmp_get_msg(mp, &msg->_msg, 1);
                if (retval < 0)
                    break;

                // block-get should never return 0
                assert(retval > 0);

                [ffpController performSelectorOnMainThread:@selector(postEvent:) withObject:msg waitUntilDone:NO];
            }
        }

        // retained in prepare_async, before SDL_CreateThreadEx
        ijkmp_dec_ref_p(&mp);
        return 0;
    }
}
````