
import React from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styled from "styled-components";

import imgCarousel from "./carousel1.jpg";
import imgCarousel3 from "./carousel3.jpg";
import imgCarousel4 from "./carousel4.jpg";
import imgCarousel5 from "./carousel5.jpg";
import imgCarousel6 from "./banner-1.png";

const CarouselContainer = styled.div`
  .carousel .slide {
    min-width: 100%;
    margin: 0;
    height: 352px !important;
    position: relative;
    text-align: center;
  }

  .carousel .control-dots {
    z-index: 0 !important;
  }
`;

const CarouselItem = styled.div`
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  height: 280px;
  margin: 70px 0;
  overflow: hidden;
  width: 85%;
  border-radius: 10px;
`;

const CarouselImage = styled.img`
  width: 100%;
  height: 100%;
  object-fit: cover; /* Ensure the image covers the entire area */
  object-position: center; /* Center the image */
`;

const CarouselImage6 = styled.img`
  width: 100%;
  height: 100%;
  object-fit: fill; /* Ensure the image covers the entire area */
  object-position: center; /* Center the image */
`;

const CarouselOverlay = styled.div`
  position: absolute;
  border-radius: 6px;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #6f36cd 0%, rgba(31, 119, 246, 0.73) 100%);
`;

const CarouselCaption = styled.div`
  position: absolute;
  top: 50%; /* Position the caption vertically in the middle */
  left: 50%; /* Position the caption horizontally in the middle */
  transform: translate(-50%, -50%); /* Center the caption both vertically and horizontally */
  text-align: center; /* Center the text within the caption */
  color: white;
  font-family: "Poppins", sans-serif; /* Apply Poppins font family */
  z-index: 1;
`;

const CaptionHeading = styled.h2`
  display: flex;
  font-weight: 600;
  font-size: 38px;
  margin: 0;
  padding: 15px;
`;

const CaptionSpan = styled.span`
  align-items: center;
  font-size: 16px;
  font-weight: 600;
  border-left: 3px solid rgba(255, 255, 255, 1);
  padding: 18px 10px;
  margin-left: 15px;
`;

const MyCarousel = ({ isModalOpen }) => {
  return (
    <CarouselContainer>
      <Carousel
        showArrows={false}
        showThumbs={false}
        showIndicators={true}
        infiniteLoop={true}
        autoPlay={true}
        interval={2000}
        stopOnHover={true}
      >
        <CarouselItem
          style={{
            background:
              "linear-gradient(90deg, #6F36CD 0%, rgba(31, 119, 246, 0.27) 100%)",
          }}
        >
          <CarouselImage src={imgCarousel} alt="Carousel 1" />
          <CarouselOverlay />
          <CarouselCaption>
            <CaptionHeading>
              AWS<CaptionSpan>Gen AI Pilots</CaptionSpan>
            </CaptionHeading>
          </CarouselCaption>
        </CarouselItem>
        <CarouselItem>
          <CarouselImage6 src={imgCarousel6} alt="Carousel 6" />
          <CarouselOverlay />
        </CarouselItem>

        <CarouselItem>
          <CarouselImage src={imgCarousel} alt="Carousel 1" />
          <CarouselOverlay />
          <CarouselCaption>
            <CaptionHeading style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </CaptionHeading>
            <p>Gen AI Pilots</p>
          </CarouselCaption>
        </CarouselItem>
        
        <CarouselItem>
          <CarouselImage src={imgCarousel3} alt="Carousel 3" />
          <CarouselOverlay />
          <CarouselCaption>
            <CaptionHeading style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </CaptionHeading>
            <p>Gen AI Pilots</p>
          </CarouselCaption>
        </CarouselItem>
        <CarouselItem>
          <CarouselImage src={imgCarousel4} alt="Carousel 4" />
          <CarouselOverlay />
          <CarouselCaption>
            <CaptionHeading style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </CaptionHeading>
            <p>Gen AI Pilots</p>
          </CarouselCaption>
        </CarouselItem>
        <CarouselItem>
          <CarouselImage src={imgCarousel5} alt="Carousel 5" />
          <CarouselOverlay />
          <CarouselCaption>
            <CaptionHeading>
              AWS EBU<CaptionSpan>Gen AI Pilots</CaptionSpan>
            </CaptionHeading>
          </CarouselCaption>
        </CarouselItem>
      </Carousel>
    </CarouselContainer>
  );
};

export default MyCarousel;
