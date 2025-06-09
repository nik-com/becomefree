
import { Card, CardContent, CardHeader } from "@/components/ui/card";
import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";
import { BookOpen } from "lucide-react";

interface CourseCardProps {
  title: string;
  description: string;
  image?: string;
  category: string;
  difficulty: 'Beginner' | 'Intermediate' | 'Advanced';
  lessonsCount: number;
}

const CourseCard = ({ title, description, image, category, difficulty, lessonsCount }: CourseCardProps) => {
  const difficultyColors = {
    Beginner: 'bg-growth/10 text-growth',
    Intermediate: 'bg-wisdom/10 text-wisdom',
    Advanced: 'bg-energy/10 text-energy'
  };

  return (
    <Card className="group overflow-hidden transition-all duration-300 hover:shadow-lg hover:scale-[1.02] animate-slide-up">
      {image && (
        <div className="h-48 overflow-hidden">
          <img 
            src={image} 
            alt={title}
            className="h-full w-full object-cover transition-transform duration-300 group-hover:scale-105"
          />
        </div>
      )}
      
      <CardHeader className="pb-3">
        <div className="flex items-start justify-between gap-2">
          <h3 className="font-semibold text-lg leading-tight group-hover:text-primary transition-colors">
            {title}
          </h3>
          <Badge variant="secondary" className="shrink-0">
            {category}
          </Badge>
        </div>
      </CardHeader>
      
      <CardContent className="pt-0">
        <p className="text-muted-foreground text-sm mb-4 leading-relaxed">
          {description}
        </p>
        
        <div className="flex items-center justify-between mb-4">
          <Badge className={difficultyColors[difficulty]}>
            {difficulty}
          </Badge>
          <span className="text-sm text-muted-foreground">
            {lessonsCount} lessons
          </span>
        </div>
        
        <Button className="w-full gradient-inspiration text-white hover:opacity-90 transition-opacity">
          <BookOpen className="h-4 w-4 mr-2" />
          Start Course
        </Button>
      </CardContent>
    </Card>
  );
};

export default CourseCard;

import { Card, CardContent } from "@/components/ui/card";

const DailyQuote = () => {
  const todaysQuote = {
    quote: "The only way to do great work is to love what you do. If you haven't found it yet, keep looking. Don't settle.",
    author: "Steve Jobs"
  };

  return (
    <Card className="relative overflow-hidden bg-gradient-to-br from-primary/5 via-peace/5 to-energy/5 border-none">
      <div className="absolute inset-0 bg-gradient-to-br from-primary/10 to-transparent opacity-50" />
      <CardContent className="relative z-10 p-8 text-center">
        <div className="mb-4">
          <span className="text-primary text-sm font-medium uppercase tracking-wider">
            Daily Inspiration
          </span>
        </div>
        
        <blockquote className="text-2xl md:text-3xl font-medium leading-relaxed text-foreground mb-6 animate-fade-in">
          "{todaysQuote.quote}"
        </blockquote>
        
        <cite className="text-muted-foreground text-lg">
          — {todaysQuote.author}
        </cite>
        
        <div className="mt-6 flex justify-center">
          <div className="w-12 h-1 bg-gradient-to-r from-primary to-peace rounded-full" />
        </div>
      </CardContent>
    </Card>
  );
};

export default DailyQuote;
import { Button } from "@/components/ui/button";
import { Link, useLocation } from "react-router-dom";
import { Home, BookOpen, List, User, Mail } from "lucide-react";

const Navigation = () => {
  const location = useLocation();

  const navItems = [
    { path: "/", label: "Home", icon: Home },
    { path: "/quotes", label: "Quotes", icon: List },
    { path: "/courses", label: "Courses", icon: BookOpen },
    { path: "/about", label: "About Me", icon: User },
    { path: "/contact", label: "Contact", icon: Mail },
  ];

  return (
    <nav className="sticky top-0 z-50 w-full border-b bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60">
      <div className="container mx-auto px-4">
        <div className="flex h-16 items-center justify-between">
          <Link to="/" className="flex items-center space-x-2">
            <div className="h-8 w-8 rounded-lg gradient-inspiration flex items-center justify-center">
              <span className="text-white font-bold text-lg">M</span>
            </div>
            <span className="font-bold text-xl text-foreground">Mindful Growth</span>
          </Link>

          <div className="hidden md:flex items-center space-x-1">
            {navItems.map((item) => {
              const Icon = item.icon;
              const isActive = location.pathname === item.path;
              
              return (
                <Button
                  key={item.path}
                  variant={isActive ? "default" : "ghost"}
                  asChild
                  className={`flex items-center space-x-2 transition-all duration-200 ${
                    isActive ? "gradient-inspiration text-white" : ""
                  }`}
                >
                  <Link to={item.path}>
                    <Icon className="h-4 w-4" />
                    <span>{item.label}</span>
                  </Link>
                </Button>
              );
            })}
          </div>

          <div className="md:hidden">
            <Button variant="ghost" size="sm">
              <List className="h-5 w-5" />
            </Button>
          </div>
        </div>
      </div>
    </nav>
  );
};

export default Navigation;
import { Card, CardContent } from "@/components/ui/card";
import { Badge } from "@/components/ui/badge";

interface QuoteCardProps {
  quote: string;
  author?: string;
  tags: string[];
  isFavorite?: boolean;
  image?: string;
}

const QuoteCard = ({ quote, author, tags, isFavorite = false, image }: QuoteCardProps) => {
  return (
    <Card className="group relative overflow-hidden transition-all duration-300 hover:shadow-lg hover:scale-[1.02] animate-fade-in">
      {image && (
        <div className="h-48 w-full overflow-hidden">
          <img 
            src={image} 
            alt="Quote background" 
            className="h-full w-full object-cover transition-transform duration-300 group-hover:scale-105"
          />
        </div>
      )}
      <CardContent className={`p-6 ${image ? 'bg-white/95 backdrop-blur-sm' : ''}`}>
        <blockquote className="text-lg font-medium leading-relaxed text-foreground mb-4">
          "{quote}"
        </blockquote>
        
        {author && (
          <cite className="text-sm text-muted-foreground block mb-4">
            — {author}
          </cite>
        )}
        
        <div className="flex flex-wrap gap-2 mb-2">
          {tags.map((tag) => (
            <Badge 
              key={tag} 
              variant="secondary" 
              className="text-xs bg-primary/10 text-primary hover:bg-primary/20"
            >
              {tag}
            </Badge>
          ))}
        </div>

        {isFavorite && (
          <div className="absolute top-4 right-4">
            <div className="w-6 h-6 rounded-full bg-wisdom flex items-center justify-center">
              <span className="text-white text-xs">★</span>
            </div>
          </div>
        )}
      </CardContent>
    </Card>
  );
};

export default QuoteCard;import Navigation from "@/components/Navigation";
import { Card, CardContent } from "@/components/ui/card";

const About = () => {
  return (
    <div className="min-h-screen bg-background">
      <Navigation />
      
      {/* Header */}
      <section className="py-16 bg-gradient-to-br from-energy/5 to-growth/10">
        <div className="container mx-auto px-4">
          <div className="text-center max-w-3xl mx-auto">
            <h1 className="text-4xl md:text-5xl font-bold text-foreground mb-4 animate-fade-in">
              About Me
            </h1>
            <p className="text-lg text-muted-foreground mb-8 animate-slide-up">
              My journey of self-discovery and why I'm passionate about helping others grow.
            </p>
          </div>
        </div>
      </section>

      {/* Main Content */}
      <section className="py-12">
        <div className="container mx-auto px-4">
          <div className="max-w-4xl mx-auto">
            <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
              {/* Profile Image */}
              <div className="lg:col-span-1">
                <Card className="overflow-hidden">
                  <CardContent className="p-0">
                    <img 
                      src="https://images.unsplash.com/photo-1649972904349-6e44c42644a7?w=400&h=500&fit=crop" 
                      alt="Profile"
                      className="w-full h-80 object-cover"
                    />
                  </CardContent>
                </Card>
              </div>

              {/* Content */}
              <div className="lg:col-span-2 space-y-6">
                <Card>
                  <CardContent className="p-6">
                    <h2 className="text-2xl font-bold text-foreground mb-4">Hello, I'm Sarah</h2>
                    <p className="text-muted-foreground leading-relaxed mb-4">
                      Welcome to my corner of the internet! I'm a certified life coach, mindfulness practitioner, 
                      and passionate advocate for personal growth. My journey into self-development began during 
                      a challenging period in my life when I realized that true change comes from within.
                    </p>
                    <p className="text-muted-foreground leading-relaxed">
                      After years of studying psychology, mindfulness, and various coaching methodologies, I've 
                      dedicated my life to helping others discover their potential and create meaningful change. 
                      This platform is my way of sharing the wisdom, tools, and inspiration that have 
                      transformed my life and the lives of countless others.
                    </p>
                  </CardContent>
                </Card>

                <Card>
                  <CardContent className="p-6">
                    <h3 className="text-xl font-semibold text-foreground mb-4">My Mission</h3>
                    <p className="text-muted-foreground leading-relaxed">
                      I believe that everyone has the power to create positive change in their life. Through 
                      carefully curated quotes, comprehensive courses, and practical guidance, I aim to provide 
                      you with the tools and inspiration you need to overcome obstacles, build confidence, 
                      and live a life aligned with your values and dreams.
                    </p>
                  </CardContent>
                </Card>

                <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                  <Card>
                    <CardContent className="p-6">
                      <h4 className="text-lg font-semibold text-foreground mb-3">Certifications</h4>
                      <ul className="text-muted-foreground space-y-2">
                        <li>• Certified Life Coach (ICF)</li>
                        <li>• Mindfulness-Based Stress Reduction</li>
                        <li>• NLP Practitioner</li>
                        <li>• Psychology Degree</li>
                      </ul>
                    </CardContent>
                  </Card>

                  <Card>
                    <CardContent className="p-6">
                      <h4 className="text-lg font-semibold text-foreground mb-3">Core Values</h4>
                      <ul className="text-muted-foreground space-y-2">
                        <li>• Authenticity & Growth</li>
                        <li>• Compassion & Understanding</li>
                        <li>• Practical Wisdom</li>
                        <li>• Continuous Learning</li>
                      </ul>
                    </CardContent>
                  </Card>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  );
};

export default About;

import Navigation from "@/components/Navigation";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { Mail, User } from "lucide-react";

const Contact = () => {
  return (
    <div className="min-h-screen bg-background">
      <Navigation />
      
      {/* Header */}
      <section className="py-16 bg-gradient-to-br from-peace/5 to-primary/10">
        <div className="container mx-auto px-4">
          <div className="text-center max-w-3xl mx-auto">
            <h1 className="text-4xl md:text-5xl font-bold text-foreground mb-4 animate-fade-in">
              Get In Touch
            </h1>
            <p className="text-lg text-muted-foreground mb-8 animate-slide-up">
              I'd love to hear from you. Whether you have questions, feedback, or just want to say hello!
            </p>
          </div>
        </div>
      </section>

      {/* Contact Form */}
      <section className="py-12">
        <div className="container mx-auto px-4">
          <div className="max-w-4xl mx-auto">
            <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
              {/* Contact Information */}
              <div className="space-y-6">
                <Card>
                  <CardHeader>
                    <CardTitle className="flex items-center gap-2">
                      <Mail className="h-5 w-5 text-primary" />
                      Let's Connect
                    </CardTitle>
                  </CardHeader>
                  <CardContent>
                    <p className="text-muted-foreground leading-relaxed mb-4">
                      I'm always excited to connect with like-minded individuals who are on their own 
                      journey of growth and self-discovery. Feel free to reach out with:
                    </p>
                    <ul className="text-muted-foreground space-y-2">
                      <li>• Questions about courses or content</li>
                      <li>• Collaboration opportunities</li>
                      <li>• Speaking engagement requests</li>
                      <li>• General feedback or suggestions</li>
                      <li>• Your own inspiring quotes to share</li>
                    </ul>
                  </CardContent>
                </Card>

                <Card>
                  <CardHeader>
                    <CardTitle>Response Time</CardTitle>
                  </CardHeader>
                  <CardContent>
                    <p className="text-muted-foreground">
                      I typically respond to messages within 24-48 hours. Thank you for your patience 
                      as I personally read and respond to every message.
                    </p>
                  </CardContent>
                </Card>
              </div>

              {/* Contact Form */}
              <Card>
                <CardHeader>
                  <CardTitle className="flex items-center gap-2">
                    <User className="h-5 w-5 text-primary" />
                    Send a Message
                  </CardTitle>
                </CardHeader>
                <CardContent className="space-y-4">
                  <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                      <label htmlFor="firstName" className="text-sm font-medium text-foreground mb-2 block">
                        First Name *
                      </label>
                      <Input id="firstName" placeholder="Your first name" required />
                    </div>
                    <div>
                      <label htmlFor="lastName" className="text-sm font-medium text-foreground mb-2 block">
                        Last Name *
                      </label>
                      <Input id="lastName" placeholder="Your last name" required />
                    </div>
                  </div>
                  
                  <div>
                    <label htmlFor="email" className="text-sm font-medium text-foreground mb-2 block">
                      Email Address *
                    </label>
                    <Input id="email" type="email" placeholder="your.email@example.com" required />
                  </div>
                  
                  <div>
                    <label htmlFor="subject" className="text-sm font-medium text-foreground mb-2 block">
                      Subject *
                    </label>
                    <Input id="subject" placeholder="What's this about?" required />
                  </div>
                  
                  <div>
                    <label htmlFor="message" className="text-sm font-medium text-foreground mb-2 block">
                      Message *
                    </label>
                    <Textarea 
                      id="message" 
                      placeholder="Tell me what's on your mind..." 
                      className="min-h-[120px]" 
                      required 
                    />
                  </div>
                  
                  <Button className="w-full gradient-inspiration text-white hover:opacity-90 transition-opacity">
                    Send Message
                  </Button>
                  
                  <p className="text-xs text-muted-foreground text-center">
                    By sending this message, you agree to be contacted via email regarding your inquiry.
                  </p>
                </CardContent>
              </Card>
            </div>
          </div>
        </div>
      </section>
    </div>
  );
};

export default Contact;import Navigation from "@/components/Navigation";
import CourseCard from "@/components/CourseCard";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { Badge } from "@/components/ui/badge";
import { Search } from "lucide-react";
import { useState } from "react";

const Courses = () => {
  const [searchTerm, setSearchTerm] = useState("");
  const [selectedCategory, setSelectedCategory] = useState("");
  const [selectedDifficulty, setSelectedDifficulty] = useState("");

  const allCourses = [
    {
      title: "Building Unshakeable Confidence",
      description: "Transform your self-doubt into unwavering confidence with practical exercises and mindset shifts that will change how you see yourself.",
      category: "Confidence",
      difficulty: "Beginner" as const,
      lessonsCount: 8,
      image: "https://images.unsplash.com/photo-1649972904349-6e44c42644a7?w=500&h=300&fit=crop"
    },
    {
      title: "Mastering Your Morning Routine",
      description: "Design a powerful morning routine that sets you up for success every single day. Learn the habits of high achievers.",
      category: "Productivity",
      difficulty: "Intermediate" as const,
      lessonsCount: 6,
      image: "https://images.unsplash.com/photo-1486312338219-ce68d2c6f44d?w=500&h=300&fit=crop"
    },
    {
      title: "The Art of Emotional Intelligence",
      description: "Develop deeper self-awareness and improve your relationships through emotional mastery and empathetic communication.",
      category: "Relationships",
      difficulty: "Advanced" as const,
      lessonsCount: 12,
      image: "https://images.unsplash.com/photo-1581091226825-a6a2a5aee158?w=500&h=300&fit=crop"
    },
    {
      title: "Mindful Meditation for Beginners",
      description: "Start your meditation journey with simple, effective techniques that you can practice anywhere, anytime.",
      category: "Mindfulness",
      difficulty: "Beginner" as const,
      lessonsCount: 10,
      image: "https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?w=500&h=300&fit=crop"
    },
    {
      title: "Goal Setting That Actually Works",
      description: "Learn the science-backed methods for setting and achieving goals that align with your values and vision.",
      category: "Goal Setting",
      difficulty: "Intermediate" as const,
      lessonsCount: 7,
      image: "https://images.unsplash.com/photo-1501854140801-50d01698950b?w=500&h=300&fit=crop"
    },
    {
      title: "Overcoming Fear and Anxiety",
      description: "Practical strategies to manage fear and anxiety, turning them into stepping stones for personal growth.",
      category: "Mental Health",
      difficulty: "Beginner" as const,
      lessonsCount: 9,
      image: "https://images.unsplash.com/photo-1518495973542-4542c06a5843?w=500&h=300&fit=crop"
    }
  ];

  const categories = Array.from(new Set(allCourses.map(course => course.category)));
  const difficulties = ['Beginner', 'Intermediate', 'Advanced'];

  const filteredCourses = allCourses.filter(course => {
    const matchesSearch = course.title.toLowerCase().includes(searchTerm.toLowerCase()) ||
                         course.description.toLowerCase().includes(searchTerm.toLowerCase());
    const matchesCategory = selectedCategory === "" || course.category === selectedCategory;
    const matchesDifficulty = selectedDifficulty === "" || course.difficulty === selectedDifficulty;
    return matchesSearch && matchesCategory && matchesDifficulty;
  });

  return (
    <div className="min-h-screen bg-background">
      <Navigation />
      
      {/* Header */}
      <section className="py-16 bg-gradient-to-br from-growth/5 to-wisdom/10">
        <div className="container mx-auto px-4">
          <div className="text-center max-w-3xl mx-auto">
            <h1 className="text-4xl md:text-5xl font-bold text-foreground mb-4 animate-fade-in">
              Self-Development Courses
            </h1>
            <p className="text-lg text-muted-foreground mb-8 animate-slide-up">
              Comprehensive courses designed to guide you through transformative personal growth experiences.
            </p>
          </div>
        </div>
      </section>

      {/* Search and Filter */}
      <section className="py-8 bg-background border-b">
        <div className="container mx-auto px-4">
          <div className="max-w-4xl mx-auto">
            <div className="flex flex-col md:flex-row gap-4 items-center mb-6">
              <div className="relative flex-1">
                <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 text-muted-foreground h-4 w-4" />
                <Input
                  placeholder="Search courses..."
                  value={searchTerm}
                  onChange={(e) => setSearchTerm(e.target.value)}
                  className="pl-10"
                />
              </div>
            </div>
            
            <div className="flex flex-col sm:flex-row gap-4">
              <div>
                <p className="text-sm text-muted-foreground mb-2">Categories:</p>
                <div className="flex flex-wrap gap-2">
                  <Badge
                    variant={selectedCategory === "" ? "default" : "secondary"}
                    className="cursor-pointer transition-all hover:scale-105"
                    onClick={() => setSelectedCategory("")}
                  >
                    All
                  </Badge>
                  {categories.map((category) => (
                    <Badge
                      key={category}
                      variant={selectedCategory === category ? "default" : "secondary"}
                      className="cursor-pointer transition-all hover:scale-105"
                      onClick={() => setSelectedCategory(selectedCategory === category ? "" : category)}
                    >
                      {category}
                    </Badge>
                  ))}
                </div>
              </div>
              
              <div>
                <p className="text-sm text-muted-foreground mb-2">Difficulty:</p>
                <div className="flex flex-wrap gap-2">
                  <Badge
                    variant={selectedDifficulty === "" ? "default" : "secondary"}
                    className="cursor-pointer transition-all hover:scale-105"
                    onClick={() => setSelectedDifficulty("")}
                  >
                    All
                  </Badge>
                  {difficulties.map((difficulty) => (
                    <Badge
                      key={difficulty}
                      variant={selectedDifficulty === difficulty ? "default" : "secondary"}
                      className="cursor-pointer transition-all hover:scale-105"
                      onClick={() => setSelectedDifficulty(selectedDifficulty === difficulty ? "" : difficulty)}
                    >
                      {difficulty}
                    </Badge>
                  ))}
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      {/* Courses Grid */}
      <section className="py-12">
        <div className="container mx-auto px-4">
          <div className="max-w-6xl mx-auto">
            <div className="mb-6 text-center">
              <p className="text-muted-foreground">
                Showing {filteredCourses.length} of {allCourses.length} courses
              </p>
            </div>
            
            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
              {filteredCourses.map((course, index) => (
                <CourseCard key={index} {...course} />
              ))}
            </div>
            
            {filteredCourses.length === 0 && (
              <div className="text-center py-12">
                <p className="text-muted-foreground text-lg">
                  No courses found matching your search criteria.
                </p>
                <Button 
                  variant="outline" 
                  className="mt-4"
                  onClick={() => {
                    setSearchTerm("");
                    setSelectedCategory("");
                    setSelectedDifficulty("");
                  }}
                >
                  Clear Filters
                </Button>
              </div>
            )}
          </div>
        </div>
      </section>
    </div>
  );
};

export default Courses;
import Navigation from "@/components/Navigation";
import QuoteCard from "@/components/QuoteCard";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { Badge } from "@/components/ui/badge";
import { Search } from "lucide-react";
import { useState } from "react";

const Quotes = () => {
  const [searchTerm, setSearchTerm] = useState("");
  const [selectedTag, setSelectedTag] = useState("");

  const allQuotes = [
    {
      quote: "Believe you can and you're halfway there.",
      author: "Theodore Roosevelt",
      tags: ["confidence", "mindset"],
      isFavorite: true
    },
    {
      quote: "The future belongs to those who believe in the beauty of their dreams.",
      author: "Eleanor Roosevelt",
      tags: ["dreams", "future", "inspiration"]
    },
    {
      quote: "It is during our darkest moments that we must focus to see the light.",
      author: "Aristotle",
      tags: ["resilience", "hope", "mindset"]
    },
    {
      quote: "Success is not final, failure is not fatal: it is the courage to continue that counts.",
      author: "Winston Churchill",
      tags: ["success", "resilience", "courage"],
      isFavorite: true
    },
    {
      quote: "The only impossible journey is the one you never begin.",
      author: "Tony Robbins",
      tags: ["motivation", "action", "journey"]
    },
    {
      quote: "Your limitation—it's only your imagination.",
      author: "Unknown",
      tags: ["limitations", "imagination", "potential"]
    },
    {
      quote: "Great things never come from comfort zones.",
      author: "Unknown",
      tags: ["comfort zone", "growth", "challenge"]
    },
    {
      quote: "Dream it. Wish it. Do it.",
      author: "Unknown",
      tags: ["dreams", "action", "motivation"]
    },
    {
      quote: "Don't wait for opportunity. Create it.",
      author: "George Bernard Shaw",
      tags: ["opportunity", "creation", "action"]
    }
  ];

  const allTags = Array.from(new Set(allQuotes.flatMap(quote => quote.tags)));

  const filteredQuotes = allQuotes.filter(quote => {
    const matchesSearch = quote.quote.toLowerCase().includes(searchTerm.toLowerCase()) ||
                         quote.author?.toLowerCase().includes(searchTerm.toLowerCase());
    const matchesTag = selectedTag === "" || quote.tags.includes(selectedTag);
    return matchesSearch && matchesTag;
  });

  return (
    <div className="min-h-screen bg-background">
      <Navigation />
      
      {/* Header */}
      <section className="py-16 bg-gradient-to-br from-primary/5 to-peace/10">
        <div className="container mx-auto px-4">
          <div className="text-center max-w-3xl mx-auto">
            <h1 className="text-4xl md:text-5xl font-bold text-foreground mb-4 animate-fade-in">
              Inspirational Quotes
            </h1>
            <p className="text-lg text-muted-foreground mb-8 animate-slide-up">
              Discover wisdom from great minds throughout history. Let these words inspire and guide your journey.
            </p>
          </div>
        </div>
      </section>

      {/* Search and Filter */}
      <section className="py-8 bg-background border-b">
        <div className="container mx-auto px-4">
          <div className="max-w-4xl mx-auto">
            <div className="flex flex-col md:flex-row gap-4 items-center mb-6">
              <div className="relative flex-1">
                <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 text-muted-foreground h-4 w-4" />
                <Input
                  placeholder="Search quotes or authors..."
                  value={searchTerm}
                  onChange={(e) => setSearchTerm(e.target.value)}
                  className="pl-10"
                />
              </div>
              <Button 
                variant={selectedTag === "" ? "default" : "outline"}
                onClick={() => setSelectedTag("")}
              >
                All Categories
              </Button>
            </div>
            
            <div className="flex flex-wrap gap-2">
              {allTags.map((tag) => (
                <Badge
                  key={tag}
                  variant={selectedTag === tag ? "default" : "secondary"}
                  className="cursor-pointer transition-all hover:scale-105"
                  onClick={() => setSelectedTag(selectedTag === tag ? "" : tag)}
                >
                  {tag}
                </Badge>
              ))}
            </div>
          </div>
        </div>
      </section>

      {/* Quotes Grid */}
      <section className="py-12">
        <div className="container mx-auto px-4">
          <div className="max-w-6xl mx-auto">
            <div className="mb-6 text-center">
              <p className="text-muted-foreground">
                Showing {filteredQuotes.length} of {allQuotes.length} quotes
              </p>
            </div>
            
            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
              {filteredQuotes.map((quote, index) => (
                <QuoteCard key={index} {...quote} />
              ))}
            </div>
            
            {filteredQuotes.length === 0 && (
              <div className="text-center py-12">
                <p className="text-muted-foreground text-lg">
                  No quotes found matching your search criteria.
                </p>
                <Button 
                  variant="outline" 
                  className="mt-4"
                  onClick={() => {
                    setSearchTerm("");
                    setSelectedTag("");
                  }}
                >
                  Clear Filters
                </Button>
              </div>
            )}
          </div>
        </div>
      </section>
    </div>
  );
};

export default Quotes;

import type { Config } from "tailwindcss";

export default {
	darkMode: ["class"],
	content: [
		"./pages/**/*.{ts,tsx}",
		"./components/**/*.{ts,tsx}",
		"./app/**/*.{ts,tsx}",
		"./src/**/*.{ts,tsx}",
	],
	prefix: "",
	theme: {
		container: {
			center: true,
			padding: '2rem',
			screens: {
				'2xl': '1400px'
			}
		},
		extend: {
			colors: {
				border: 'hsl(var(--border))',
				input: 'hsl(var(--input))',
				ring: 'hsl(var(--ring))',
				background: 'hsl(var(--background))',
				foreground: 'hsl(var(--foreground))',
				primary: {
					DEFAULT: 'hsl(var(--primary))',
					foreground: 'hsl(var(--primary-foreground))'
				},
				secondary: {
					DEFAULT: 'hsl(var(--secondary))',
					foreground: 'hsl(var(--secondary-foreground))'
				},
				destructive: {
					DEFAULT: 'hsl(var(--destructive))',
					foreground: 'hsl(var(--destructive-foreground))'
				},
				muted: {
					DEFAULT: 'hsl(var(--muted))',
					foreground: 'hsl(var(--muted-foreground))'
				},
				accent: {
					DEFAULT: 'hsl(var(--accent))',
					foreground: 'hsl(var(--accent-foreground))'
				},
				popover: {
					DEFAULT: 'hsl(var(--popover))',
					foreground: 'hsl(var(--popover-foreground))'
				},
				card: {
					DEFAULT: 'hsl(var(--card))',
					foreground: 'hsl(var(--card-foreground))'
				},
				sidebar: {
					DEFAULT: 'hsl(var(--sidebar-background))',
					foreground: 'hsl(var(--sidebar-foreground))',
					primary: 'hsl(var(--sidebar-primary))',
					'primary-foreground': 'hsl(var(--sidebar-primary-foreground))',
					accent: 'hsl(var(--sidebar-accent))',
					'accent-foreground': 'hsl(var(--sidebar-accent-foreground))',
					border: 'hsl(var(--sidebar-border))',
					ring: 'hsl(var(--sidebar-ring))'
				},
				inspiration: 'hsl(var(--inspiration))',
				wisdom: 'hsl(var(--wisdom))',
				growth: 'hsl(var(--growth))',
				peace: 'hsl(var(--peace))',
				energy: 'hsl(var(--energy))'
			},
			borderRadius: {
				lg: 'var(--radius)',
				md: 'calc(var(--radius) - 2px)',
				sm: 'calc(var(--radius) - 4px)'
			},
			keyframes: {
				'accordion-down': {
					from: {
						height: '0'
					},
					to: {
						height: 'var(--radix-accordion-content-height)'
					}
				},
				'accordion-up': {
					from: {
						height: 'var(--radix-accordion-content-height)'
					},
					to: {
						height: '0'
					}
				},
				'fade-in': {
					'0%': {
						opacity: '0',
						transform: 'translateY(10px)'
					},
					'100%': {
						opacity: '1',
						transform: 'translateY(0)'
					}
				},
				'slide-up': {
					'0%': {
						opacity: '0',
						transform: 'translateY(30px)'
					},
					'100%': {
						opacity: '1',
						transform: 'translateY(0)'
					}
				},
				'float': {
					'0%, 100%': {
						transform: 'translateY(0px)'
					},
					'50%': {
						transform: 'translateY(-10px)'
					}
				}
			},
			animation: {
				'accordion-down': 'accordion-down 0.2s ease-out',
				'accordion-up': 'accordion-up 0.2s ease-out',
				'fade-in': 'fade-in 0.6s ease-out',
				'slide-up': 'slide-up 0.8s ease-out',
				'float': 'float 3s ease-in-out infinite'
			}
		}
	},
	plugins: [require("tailwindcss-animate")],
} satisfies Config;
import { Toaster } from "@/components/ui/toaster";
import { Toaster as Sonner } from "@/components/ui/sonner";
import { TooltipProvider } from "@/components/ui/tooltip";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Index from "./pages/Index";
import Quotes from "./pages/Quotes";
import Courses from "./pages/Courses";
import About from "./pages/About";
import Contact from "./pages/Contact";
import NotFound from "./pages/NotFound";

const queryClient = new QueryClient();

const App = () => (
  <QueryClientProvider client={queryClient}>
    <TooltipProvider>
      <Toaster />
      <Sonner />
      <BrowserRouter>
        <Routes>
          <Route path="/" element={<Index />} />
          <Route path="/quotes" element={<Quotes />} />
          <Route path="/courses" element={<Courses />} />
          <Route path="/about" element={<About />} />
          <Route path="/contact" element={<Contact />} />
          {/* ADD ALL CUSTOM ROUTES ABOVE THE CATCH-ALL "*" ROUTE */}
          <Route path="*" element={<NotFound />} />
        </Routes>
      </BrowserRouter>
    </TooltipProvider>
  </QueryClientProvider>
);

export default App;@tailwind base;
@tailwind components;
@tailwind utilities;

/* Definition of the design system. All colors, gradients, fonts, etc should be defined here. */

@layer base {
  :root {   --background: 0 0% 100%;
    --foreground: 222.2 84% 4.9%;

    --card: 0 0% 100%;
    --card-foreground: 222.2 84% 4.9%;

    --popover: 0 0% 100%;
    --popover-foreground: 222.2 84% 4.9%;

    --primary: 222.2 47.4% 11.2%;
    --primary-foreground: 210 40% 98%;

    --secondary: 210 40% 96.1%;
    --secondary-foreground: 222.2 47.4% 11.2%;

    --muted: 210 40% 96.1%;
    --muted-foreground: 215.4 16.3% 46.9%;

    --accent: 210 40% 96.1%;
    --accent-foreground: 222.2 47.4% 11.2%;

    --destructive: 0 84.2% 60.2%;
    --destructive-foreground: 210 40% 98%;

    --border: 214.3 31.8% 91.4%;
    --input: 214.3 31.8% 91.4%;
    --ring: 222.2 84% 4.9%;

    --radius: 0.5rem;

    --sidebar-background: 0 0% 98%;

    --sidebar-foreground: 240 5.3% 26.1%;

    --sidebar-primary: 240 5.9% 10%;

    --sidebar-primary-foreground: 0 0% 98%;

    --sidebar-accent: 240 4.8% 95.9%;

    --sidebar-accent-foreground: 240 5.9% 10%;

    --sidebar-border: 220 13% 91%;

    --sidebar-ring: 217.2 91.2% 59.8%;
  }

  .dark {
    --background: 222.2 84% 4.9%;
    --foreground: 210 40% 98%;

    --card: 222.2 84% 4.9%;
    --card-foreground: 210 40% 98%;

    --popover: 222.2 84% 4.9%;
    --popover-foreground: 210 40% 98%;

    --primary: 210 40% 98%;
    --primary-foreground: 222.2 47.4% 11.2%;

    --secondary: 217.2 32.6% 17.5%;
    --secondary-foreground: 210 40% 98%;

    --muted: 217.2 32.6% 17.5%;
    --muted-foreground: 215 20.2% 65.1%;

    --accent: 217.2 32.6% 17.5%;
    --accent-foreground: 210 40% 98%;
    --background: 251 100% 99%; --foreground: 240 10% 15%;

    --card: 0 0% 100%;
    --card-foreground: 240 10% 15%;

    --popover: 0 0% 100%;
    --popover-foreground: 240 10% 15%;

    --primary: 217 91% 60%;
    --primary-foreground: 0 0% 98%;

    --secondary: 220 14% 96%;
    --secondary-foreground: 240 10% 15%;

    --muted: 220 14% 96%;
    --muted-foreground: 215 16% 47%;

    --accent: 220 14% 96%;
    --accent-foreground: 240 10% 15%;

    --destructive: 0 84% 60%;
    --destructive-foreground: 210 40% 98%;

    --border: 220 13% 91%;
    --input: 220 13% 91%;
    --ring: 217 91% 60%;

    --radius: 0.75rem;

    --sidebar-background: 0 0% 98%;
    --sidebar-foreground: 240 5.3% 26.1%;
    --sidebar-primary: 240 5.9% 10%;
    --sidebar-primary-foreground: 0 0% 98%;
    --sidebar-accent: 240 4.8% 95.9%;
    --sidebar-accent-foreground: 240 5.9% 10%;
    --sidebar-border: 220 13% 91%;
    --sidebar-ring: 217.2 91.2% 59.8%;

    /* Custom design system colors */
    --inspiration: 217 91% 60%;
    --wisdom: 45 93% 47%;
    --growth: 142 76% 36%;
    --peace: 200 100% 70%;
    --energy: 280 100% 70%;
  }

  .dark {
    --background: 240 10% 4%;
    --foreground: 220 14% 96%;

    --card: 240 10% 4%;
    --card-foreground: 220 14% 96%;

    --popover: 240 10% 4%;
    --popover-foreground: 220 14% 96%;

    --primary: 217 91% 60%;
    --primary-foreground: 240 10% 4%;

    --secondary: 240 3.7% 15.9%;
    --secondary-foreground: 220 14% 96%;

    --muted: 240 3.7% 15.9%;
    --muted-foreground: 240 5% 64.9%;

    --accent: 240 3.7% 15.9%;
    --accent-foreground: 220 14% 96%;

    --destructive: 0 62.8% 30.6%;
    --destructive-foreground: 210 40% 98%;

    --border: 240 3.7% 15.9%;
    --input: 240 3.7% 15.9%;
    --ring: 217 91% 60%;

    --sidebar-background: 240 5.9% 10%;
    --sidebar-foreground: 240 4.8% 95.9%;
    --sidebar-primary: 224.3 76.3% 48%;
    --sidebar-primary-foreground: 0 0% 100%;
    --sidebar-accent: 240 3.7% 15.9%;
    --sidebar-accent-foreground: 240 4.8% 95.9%;
    --sidebar-border: 240 3.7% 15.9%;
    --sidebar-ring: 217.2 91.2% 59.8%;
  }
}

@layer base {
  * {
    @apply border-border;
  }

  body {
    @apply bg-background text-foreground;
  }
}

@layer utilities {
  .gradient-inspiration {
    background: linear-gradient(135deg, hsl(var(--inspiration)) 0%, hsl(var(--peace)) 100%);
  }
  
  .gradient-wisdom {
    background: linear-gradient(135deg, hsl(var(--wisdom)) 0%, hsl(var(--energy)) 100%);
  }
  
  .gradient-growth {
    background: linear-gradient(135deg, hsl(var(--growth)) 0%, hsl(var(--inspiration)) 100%);
  }

  .text-inspiration {
    color: hsl(var(--inspiration));
  }
  
  .text-wisdom {
    color: hsl(var(--wisdom));
  }
  
  .text-growth {
    color: hsl(var(--growth));
  }

  .animate-fade-in {
    animation: fadeIn 0.6s ease-out;
  }

  .animate-slide-up {
    animation: slideUp 0.8s ease-out;
  }

  .animate-float {
    animation: float 3s ease-in-out infinite;
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes float {
  0%, 100% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-10px);
  }
}const Index = () => {
  return (
    <div className="min-h-screen flex items-center justify-center bg-background">
      <div className="text-center">
        <h1 className="text-4xl font-bold mb-4">Welcome to Your Blank App</h1>
        <p className="text-xl text-muted-foreground">Start building your amazing project here!</p>
      </div>

import Navigation import Navigation from "@/components/Navigation";
import DailyQuote from "@/components/DailyQuote";
import QuoteCard from "@/components/QuoteCard";
import CourseCard from "@/components/CourseCard";
import { Button } from "@/components/ui/button";
import { ArrowDown } from "lucide-react";

const Index = () => {
  const featuredQuotes = [
    {
      quote: "Believe you can and you're halfway there.",
      author: "Theodore Roosevelt",
      tags: ["confidence", "mindset"],
      isFavorite: true
    },
    {
      quote: "The future belongs to those who believe in the beauty of their dreams.",
      author: "Eleanor Roosevelt",
      tags: ["dreams", "future", "inspiration"]
    },
    {
      quote: "It is during our darkest moments that we must focus to see the light.",
      author: "Aristotle",
      tags: ["resilience", "hope", "mindset"]
    }
  ];

  const featuredCourses = [
    {
      title: "Building Unshakeable Confidence",
      description: "Transform your self-doubt into unwavering confidence with practical exercises and mindset shifts.",
      category: "Confidence",
      difficulty: "Beginner" as const,
      lessonsCount: 8,
      image: "https://images.unsplash.com/photo-1649972904349-6e44c42644a7?w=500&h=300&fit=crop"
    },
    {
      title: "Mastering Your Morning Routine",
      description: "Design a powerful morning routine that sets you up for success every single day.",
      category: "Productivity",
      difficulty: "Intermediate" as const,
      lessonsCount: 6,
      image: "https://images.unsplash.com/photo-1486312338219-ce68d2c6f44d?w=500&h=300&fit=crop"
    },
    {
      title: "The Art of Emotional Intelligence",
      description: "Develop deeper self-awareness and improve your relationships through emotional mastery.",
      category: "Relationships",
      difficulty: "Advanced" as const,
      lessonsCount: 12,
      image: "https://images.unsplash.com/photo-1581091226825-a6a2a5aee158?w=500&h=300&fit=crop"
    }
  ];

  return (
    <div className="min-h-screen bg-background">
      <Navigation />
      
      {/* Hero Section */}
      <section className="relative overflow-hidden bg-gradient-to-br from-background via-primary/5 to-peace/10">
        <div className="container mx-auto px-4 py-16 md:py-24">
          <div className="text-center max-w-4xl mx-auto">
            <h1 className="text-4xl md:text-6xl lg:text-7xl font-bold text-foreground mb-6 animate-fade-in">
              Transform Your
              <span className="bg-gradient-to-r from-primary via-peace to-energy bg-clip-text text-transparent"> Mindset</span>
            </h1>
            
            <p className="text-xl md:text-2xl text-muted-foreground mb-8 leading-relaxed animate-slide-up">
              Discover wisdom, build habits, and unlock your potential with our curated collection of motivational content and transformative courses.
            </p>
            
            <div className="flex flex-col sm:flex-row gap-4 justify-center items-center mb-12 animate-slide-up">
              <Button size="lg" className="gradient-inspiration text-white hover:opacity-90 transition-opacity px-8">
                Explore Quotes
              </Button>
              <Button size="lg" variant="outline" className="px-8">
                Browse Courses
              </Button>
            </div>
            
            <div className="animate-float">
              <ArrowDown className="h-6 w-6 text-muted-foreground mx-auto" />
            </div>
          </div>
        </div>
      </section>

      {/* Daily Quote Section */}
      <section className="py-16 bg-background">
        <div className="container mx-auto px-4">
          <DailyQuote />
        </div>
      </section>

      {/* Featured Quotes */}
      <section className="py-16 bg-muted/30">
        <div className="container mx-auto px-4">
          <div className="text-center mb-12">
            <h2 className="text-3xl md:text-4xl font-bold text-foreground mb-4">
              Featured Quotes
            </h2>
            <p className="text-lg text-muted-foreground max-w-2xl mx-auto">
              Handpicked wisdom to inspire your journey of growth and self-discovery
            </p>
          </div>
          
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            {featuredQuotes.map((quote, index) => (
              <QuoteCard key={index} {...quote} />
            ))}
          </div>
          
          <div className="text-center mt-8">
            <Button variant="outline" size="lg">
              View All Quotes
            </Button>
          </div>
        </div>
      </section>

      {/* Featured Courses */}
      <section className="py-16 bg-background">
        <div className="container mx-auto px-4">
          <div className="text-center mb-12">
            <h2 className="text-3xl md:text-4xl font-bold text-foreground mb-4">
              Featured Courses
            </h2>
            <p className="text-lg text-muted-foreground max-w-2xl mx-auto">
              Comprehensive guides to help you build lasting positive change in your life
            </p>
          </div>
          
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            {featuredCourses.map((course, index) => (
              <CourseCard key={index} {...course} />
            ))}
          </div>
          
          <div className="text-center mt-8">
            <Button variant="outline" size="lg">
              Browse All Courses
            </Button>
          </div>
        </div>
      </section>

      {/* CTA Section */}
      <section className="py-16 bg-gradient-to-r from-primary/10 via-peace/10 to-energy/10">
        <div className="container mx-auto px-4 text-center">
          <h2 className="text-3xl md:text-4xl font-bold text-foreground mb-4">
            Start Your Growth Journey Today
          </h2>
          <p className="text-lg text-muted-foreground mb-8 max-w-2xl mx-auto">
            Join thousands of others who are transforming their lives through mindful growth and daily inspiration.
          </p>
          <Button size="lg" className="gradient-inspiration text-white hover:opacity-90 transition-opacity px-8">
            Get Started
          </Button>
        </div>
      </section>

      {/* Footer */}
      <footer className="bg-foreground text-background py-12">
        <div className="container mx-auto px-4">
          <div className="text-center">
            <div className="flex items-center justify-center space-x-2 mb-4">
              <div className="h-8 w-8 rounded-lg gradient-inspiration flex items-center justify-center">
                <span className="text-white font-bold text-lg">M</span>
              </div>
              <span className="font-bold text-xl">Mindful Growth</span>
            </div>
            <p className="text-background/70 mb-4">
              Empowering your journey of self-discovery and personal transformation.
            </p>
            <p className="text-background/50 text-sm">
              © 2024 Mindful Growth. All rights reserved.
            </p>
          </div>
        </div>
      </footer>
    </div>
  );
};

export default Index;
