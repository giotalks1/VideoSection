import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Play } from "lucide-react";

const videos = [
  {
    title: "Final Countdown: Last 40 years History PYQs for CSE 2025",
    language: "Hinglish",
    subject: "History",
    instructor: "Arti Chhawari",
    videoUrl: "https://www.youtube.com/embed/AxNEWA9_sJ0",
    thumbnail: "https://cdn.pixabay.com/photo/2025/03/30/22/32/22-32-05-32__340.png",
    duration: "12:30",
    views: "1.2M",
  },
  // Add more video objects here
];

export default function VideoSection() {
  const [playingVideo, setPlayingVideo] = useState(null);
  const [loading, setLoading] = useState(false);
  const [showMore, setShowMore] = useState(false);

  const handlePlay = (index) => {
    setLoading(true);
    setPlayingVideo(index);
  };

  return (
    <div className="p-6 grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 gap-4 bg-gray-900 text-white">
      {videos.slice(0, showMore ? videos.length : 4).map((video, index) => (
        <Card key={index} className="rounded-2xl overflow-hidden shadow-md bg-gray-800">
          {playingVideo === index ? (
            <iframe
              className="w-full h-48"  // Increased height of iframe
              src={video.videoUrl}
              title={video.title}
              frameBorder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowFullScreen
              onLoad={() => setLoading(false)}
            ></iframe>
          ) : (
            <div
              className="w-full h-32 object-cover cursor-pointer bg-cover bg-center"
              style={{
                backgroundImage: `url(${video.thumbnail})`,
                objectFit: "contain", // Preserve the original aspect ratio of the thumbnail
              }}
              onClick={() => handlePlay(index)}
            >
              {loading && playingVideo === index && (
                <div className="flex justify-center items-center w-full h-full bg-black opacity-50">
                  <div className="spinner-border text-white" role="status">
                    <span className="sr-only">Loading...</span>
                  </div>
                </div>
              )}
            </div>
          )}
          <CardContent className="p-4">
            <div className="text-sm text-blue-400 font-semibold">
              {video.language} | {video.subject.toUpperCase()}
            </div>
            <h3 className="text-lg font-bold mt-2 text-white">{video.title}</h3>
            <p className="text-gray-300 text-sm mt-1">{video.instructor}</p>
            <div className="flex justify-between text-gray-400 mt-2 text-sm">
              <span>{video.duration}</span>
              <span>{video.views}</span>
            </div>
            <Button
              className="mt-3 flex items-center gap-2 border-2 border-blue-700 hover:border-blue-800 text-blue-700 hover:text-blue-800"
              onClick={() => handlePlay(index)}
            >
              <Play size={16} /> Watch Now
            </Button>
          </CardContent>
        </Card>
      ))}
      <div className="col-span-full flex justify-center mt-4">
        <Button
          className="bg-blue-700 hover:bg-blue-800 text-white"
          onClick={() => setShowMore(!showMore)}
        >
          {showMore ? "Show Less" : "Show More"}
        </Button>
      </div>
    </div>
  );
}
