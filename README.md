# rust-modbus-rtu
Modbus RTU crate for rust

**Note**: This crate is not yet complete. Only packet generation works.

### Usage
``` rs
use modbus_rtu::packet::{Request, RequestError, Response};

fn main() {
    // Create a new request
    let read_sensor_req: Request = Request::ReadInputRegisters {
        slave: 0x01,
        base_address: 0x0001,
        quantity: 1
    };

    // Generate packet from the request
    let packet: Vec<u8> = read_sensor_req.to_bytes().expect("fail");

    // Somewhere to read and write
    let port = ...;

    // Send packet
    let _ = port.write_all(&packet);

    // Read response
    let mut response = Response::from_request(read_sensor_req).unwrap();
    let _ = port.read_exact(&mut response.buffer);
    // response.analyze() <- WIP...
}
```