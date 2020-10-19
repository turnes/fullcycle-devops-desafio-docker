FROM golang:alpine as builder

WORKDIR /go/src/app
COPY . .

RUN go build -v main.go

FROM scratch

WORKDIR /app

COPY --from=builder /go/src/app/main .

CMD [ "./main" ]
