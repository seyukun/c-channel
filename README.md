# c thread channel
This provides a variable in C that can be used like channel in GO.

# example
```c
void  *thread_func1(t_channel *channel)
{
  usleep(1000000);
  channel_send(*channel, (int []){1000});
  return (NULL);
}

void  *thread_func2(t_channel *channel)
{
  int  data;

  data = 0;
  while (!data)
  {
    channel_recv(*channel, &data);
    if (data != 0)
      printf("%s: received data \"%d\"\n", __func__, data);
    usleep(10);
  }
  channel_send(*channel, (int []){2000});
  return (NULL);
}

int main(void)
{
  pthread_t  thread_1, thread_2;
  t_channel  channel;
  int        data;

  if (!channel_safe_create(&channel, (int []){0}, sizeof(int))
    return (1);
  pthread_create(&thread_1, 0, (void *)(void *)thread_func1, &channel);
  pthread_create(&thread_2, 0, (void *)(void *)thread_func2, &channel);
  pthread_join(thread_1, NULL);
  pthread_join(thread_2, NULL);
  channel_recv(channel, &data);
  printf("%s: received data \"%d\"\n", __func__, data);
  channel_destroy(channel);
  return (0);
}
```
