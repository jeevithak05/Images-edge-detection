# Images-edge-detection
This project implements edge detection in digital images using the Sobel operator, a popular method in image processing for highlighting object boundaries.
module sobel_edge (
    input clk,
    input [7:0] pixel_in,        // grayscale pixel input
    output reg [7:0] pixel_out   // edge detected pixel
);

reg [7:0] window[0:8]; // 3x3 window
integer i;
reg signed [10:0] Gx, Gy, G;

always @(posedge clk) begin
    // Shift pixels (for 3x3 window simulation)
    for (i=0; i<8; i=i+1)
        window[i] <= window[i+1];
    window[8] <= pixel_in;

    // Apply Sobel Masks
    Gx = (-1*window[0]) + ( 0*window[1]) + ( 1*window[2]) +
         (-2*window[3]) + ( 0*window[4]) + ( 2*window[5]) +
         (-1*window[6]) + ( 0*window[7]) + ( 1*window[8]);

    Gy = (-1*window[0]) + (-2*window[1]) + (-1*window[2]) +
         ( 0*window[3]) + ( 0*window[4]) + ( 0*window[5]) +
         ( 1*window[6]) + ( 2*window[7]) + ( 1*window[8]);

    // Gradient magnitude (approximate)
    G = ( (Gx<0) ? -Gx : Gx ) + ( (Gy<0) ? -Gy : Gy );

    // Normalize to 8-bit pixel
    if (G > 255)
        pixel_out <= 8'hFF;
    else
        pixel_out <= G[7:0];
end

endmodule
