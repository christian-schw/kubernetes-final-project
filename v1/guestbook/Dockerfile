FROM golang:1.18 as builder
WORKDIR /app
COPY main.go .
RUN go mod init guestbook
RUN go mod tidy
RUN go build -o main main.go


FROM ubuntu:18.04
# Set here working directory as well. Otherwise, build fails.
WORKDIR /app
COPY --from=builder /app/main /app/guestbook
COPY public/index.html /app/public/index.html
COPY public/script.js /app/public/script.js
COPY public/style.css /app/public/style.css
COPY public/jquery.min.js /app/public/jquery.min.js


EXPOSE 3000

CMD ["./guestbook"]