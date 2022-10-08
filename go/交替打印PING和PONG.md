```go
chPing, chPong := make(chan bool), make(chan bool)
wg := sync.WaitGroup{}
wg.Add(2)
go func() {
	for {
		<-chPing
		fmt.Println("PING")
		chPong <- true
	}
}()

go func() {
	for {
		<-chPong
		fmt.Println("PONG")
		chPing <- true
    }
}()
chPing <- true
wg.Wait()
```