---
layout: post
title: Linux 信号处理
category: LINUX
tags: Linux
keywords: 
description: 
---

方式一:

```
#include <pthread.h>
static void * sig_thread(void *arg)
{
    sigset_t *set = (sigset_t *) arg;
    int s, sig;

    for (;;) {
        s = sigwait(set, &sig);
        if (s == 0)
            DBG("[sig_thread] Signal handling thread got signal %d\n", sig);
    }
}

int main()
{
	pthread_t thread;
    sigset_t signal_set;
    int status;
    sigemptyset(&signal_set);
    sigaddset(&signal_set, SIGTERM);
    status = pthread_sigmask(SIG_BLOCK, &signal_set, NULL);
    if(0 != status)
    {
        DBG("\n");
    }
    pthread_create(&thread, NULL, &sig_thread, (void *) &signal_set);
    
    ...

}
    

```

方式二:

```
void handle_pipe(int sig)
{
    DBG("~~~~~~~~~~~~~~~sig = %d\n", sig);
}

int main()
{
   struct sigaction action;
   action.sa_handler = handle_pipe;
   sigemptyset(&action.sa_mask);
   action.sa_flags = 0;
   sigaction(SIGPIPE, &action, NULL);
   sigaction(SIGINT, &action, NULL);
   sigaction(SIGTERM, &action, NULL);
   sigaction(SIGTTOU, &action, NULL);
   sigaction(SIGTTIN, &action, NULL);
   sigaction(SIGTSTP, &action, NULL);
   sigaction(SIGHUP, &action, NULL);
   sigaction(SIGUSR1, &action, NULL);
       
    ...

}
 
```
