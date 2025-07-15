
import { useState } from "react";
import { SidebarProvider } from "@/components/ui/sidebar";
import { AppSidebar } from "@/components/AppSidebar";
import { SidebarInset } from "@/components/ui/sidebar";
import { TopBar } from "@/components/TopBar";
import { Card, CardContent } from "@/components/ui/card";
import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
import { Clock, ExternalLink, ThumbsUp, MessageCircle, Share2 } from "lucide-react";

const newsItems = [
  {
    id: 1,
    title: "WHO Announces New Guidelines for Antimicrobial Resistance",
    excerpt: "The World Health Organization has released updated guidelines for combating antimicrobial resistance in healthcare settings worldwide.",
    type: "article",
    source: "World Health Organization",
    publishDate: "2024-05-08",
    readTime: "5 min read",
    url: "#",
    image: "/placeholder.svg",
    likes: 124,
    comments: 18,
    category: "Guidelines"
  },
  {
    id: 2,
    title: "Breaking Research: New Vaccine Shows Promise Against Multiple Strains",
    excerpt: "Recent clinical trials demonstrate significant efficacy of a novel vaccine approach targeting multiple infectious disease strains.",
    type: "blog",
    source: "IDA Research Blog",
    publishDate: "2024-05-07",
    readTime: "8 min read",
    url: "#",
    image: "/placeholder.svg",
    likes: 89,
    comments: 23,
    category: "Research"
  },
  {
    id: 3,
    title: "Expert Interview: Managing Infectious Diseases in Resource-Limited Settings",
    excerpt: "Dr. Maria Santos discusses innovative approaches to infectious disease management in developing countries.",
    type: "interview",
    source: "Medical News Today",
    publishDate: "2024-05-06",
    readTime: "12 min read",
    url: "#",
    image: "/placeholder.svg",
    likes: 67,
    comments: 15,
    category: "Interview"
  },
  {
    id: 4,
    title: "Global Health Alert: Emerging Infectious Disease Outbreak",
    excerpt: "Health authorities issue alert for monitoring and response to newly identified infectious disease outbreak in Southeast Asia.",
    type: "repost",
    source: "CDC Health Alert",
    publishDate: "2024-05-05",
    readTime: "3 min read",
    url: "#",
    image: "/placeholder.svg",
    likes: 201,
    comments: 45,
    category: "Alert"
  },
  {
    id: 5,
    title: "Video Series: Advanced Diagnostic Techniques in Infectious Diseases",
    excerpt: "Educational video series covering cutting-edge diagnostic methods for rapid identification of infectious pathogens.",
    type: "video",
    source: "IDA Education",
    publishDate: "2024-05-04",
    readTime: "25 min watch",
    url: "#",
    image: "/placeholder.svg",
    likes: 156,
    comments: 32,
    category: "Education"
  },
  {
    id: 6,
    title: "Research Portal Update: New Database Features",
    excerpt: "Enhanced search capabilities and data visualization tools now available in the infectious diseases research portal.",
    type: "website",
    source: "IDA Research Portal",
    publishDate: "2024-05-03",
    readTime: "4 min read",
    url: "#",
    image: "/placeholder.svg",
    likes: 43,
    comments: 8,
    category: "Technology"
  }
];

const getTypeIcon = (type: string) => {
  const colors = {
    article: "bg-blue-100 text-blue-800",
    blog: "bg-green-100 text-green-800",
    video: "bg-red-100 text-red-800",
    website: "bg-purple-100 text-purple-800",
    repost: "bg-yellow-100 text-yellow-800",
    interview: "bg-indigo-100 text-indigo-800"
  };
  return colors[type as keyof typeof colors] || colors.article;
};

const NewsCard = ({ item }: { item: typeof newsItems[0] }) => {
  const [liked, setLiked] = useState(false);
  const [likeCount, setLikeCount] = useState(item.likes);

  const handleLike = () => {
    setLiked(!liked);
    setLikeCount(prev => liked ? prev - 1 : prev + 1);
  };

  return (
    <Card className="bg-white border-slate-200 hover:shadow-lg transition-shadow">
      <CardContent className="p-6">
        <div className="flex items-start gap-4">
          <img 
            src={item.image} 
            alt={item.title}
            className="w-24 h-24 object-cover rounded-lg flex-shrink-0"
          />
          
          <div className="flex-1">
            <div className="flex items-center gap-2 mb-2">
              <Badge className={getTypeIcon(item.type)}>
                {item.type.charAt(0).toUpperCase() + item.type.slice(1)}
              </Badge>
              <Badge variant="outline">{item.category}</Badge>
            </div>
            
            <h3 className="text-lg font-semibold text-slate-900 mb-2 line-clamp-2">
              {item.title}
            </h3>
            
            <p className="text-slate-600 text-sm mb-3 line-clamp-2">
              {item.excerpt}
            </p>
            
            <div className="flex items-center text-xs text-slate-500 mb-4">
              <span>{item.source}</span>
              <span className="mx-2">•</span>
              <Clock className="w-3 h-3 mr-1" />
              <span>{item.readTime}</span>
              <span className="mx-2">•</span>
              <span>{new Date(item.publishDate).toLocaleDateString()}</span>
            </div>
            
            <div className="flex items-center justify-between">
              <div className="flex items-center space-x-4">
                <Button 
                  variant="ghost" 
                  size="sm" 
                  onClick={handleLike}
                  className={`p-2 h-auto ${liked ? 'text-red-500' : 'text-slate-500'}`}
                >
                  <ThumbsUp className="w-4 h-4 mr-1" />
                  {likeCount}
                </Button>
                
                <Button variant="ghost" size="sm" className="text-slate-500 p-2 h-auto">
                  <MessageCircle className="w-4 h-4 mr-1" />
                  {item.comments}
                </Button>
                
                <Button variant="ghost" size="sm" className="text-slate-500 p-2 h-auto">
                  <Share2 className="w-4 h-4" />
                </Button>
              </div>
              
              <Button variant="outline" size="sm">
                <ExternalLink className="w-4 h-4 mr-1" />
                Read More
              </Button>
            </div>
          </div>
        </div>
      </CardContent>
    </Card>
  );
};

export default function News() {
  const [selectedType, setSelectedType] = useState("all");
  
  const filteredNews = selectedType === "all" 
    ? newsItems 
    : newsItems.filter(item => item.type === selectedType);

  return (
    <SidebarProvider>
      <div className="min-h-screen flex w-full bg-gradient-to-br from-blue-50 to-white">
        <AppSidebar />
        <SidebarInset>
          <TopBar />
          <div className="p-6">
            <div className="mb-6">
              <h1 className="text-3xl font-bold text-slate-900 mb-2">News & Updates</h1>
              <p className="text-slate-600">Stay informed with the latest in infectious diseases</p>
            </div>

            <Tabs defaultValue="all" className="space-y-6">
              <TabsList className="grid w-full grid-cols-7 max-w-4xl">
                <TabsTrigger value="all">All</TabsTrigger>
                <TabsTrigger value="article">Articles</TabsTrigger>
                <TabsTrigger value="blog">Blog</TabsTrigger>
                <TabsTrigger value="video">Videos</TabsTrigger>
                <TabsTrigger value="interview">Interviews</TabsTrigger>
                <TabsTrigger value="repost">Reposts</TabsTrigger>
                <TabsTrigger value="website">Website</TabsTrigger>
              </TabsList>

              <TabsContent value="all" className="space-y-6">
                <div className="space-y-4">
                  {newsItems.map((item) => (
                    <NewsCard key={item.id} item={item} />
                  ))}
                </div>
              </TabsContent>

              {['article', 'blog', 'video', 'interview', 'repost', 'website'].map((type) => (
                <TabsContent key={type} value={type} className="space-y-6">
                  <div className="space-y-4">
                    {newsItems.filter(item => item.type === type).map((item) => (
                      <NewsCard key={item.id} item={item} />
                    ))}
                  </div>
                </TabsContent>
              ))}
            </Tabs>
          </div>
        </SidebarInset>
      </div>
    </SidebarProvider>
  );
}

